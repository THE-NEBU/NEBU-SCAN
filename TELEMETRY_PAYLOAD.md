# Telemetry Payload Documentation

## Endpoint
```
POST https://api.thenebu.com/api/cli/telemetry
Content-Type: application/json
```

## Payload Structure

### Waste Optimization Scan Completed
```json
{
  "anonymousId": "550e8400-e29b-41d4-a716-446655440000",
  "version": "2.0.0",
  "os": "darwin",
  "arch": "amd64",
  "event": "scan_completed",
  "timestamp": "2025-10-22T14:30:00Z",
  "durationMs": 45000,
  "cloudProvider": "aws",
  "serviceType": "waste",
  "regionsCount": 17,
  "findings": {
    "waste": {
      "critical": 25,
      "high": 48,
      "medium": 156,
      "low": 89,
      "total": 318,
      "savings": 4250.50
    }
  }
}
```

### Security Audit Scan Completed
```json
{
  "anonymousId": "550e8400-e29b-41d4-a716-446655440000",
  "version": "2.0.0",
  "os": "linux",
  "arch": "amd64",
  "event": "scan_completed",
  "timestamp": "2025-10-22T14:35:00Z",
  "durationMs": 138000,
  "cloudProvider": "aws",
  "serviceType": "security",
  "regionsCount": 17,
  "findings": {
    "security": {
      "critical": 165,
      "high": 464,
      "medium": 2341,
      "low": 399,
      "total": 3369
    }
  }
}
```

### Scan Failed
```json
{
  "anonymousId": "550e8400-e29b-41d4-a716-446655440000",
  "version": "2.0.0",
  "os": "windows",
  "arch": "amd64",
  "event": "scan_failed",
  "timestamp": "2025-10-22T14:40:00Z",
  "cloudProvider": "aws",
  "serviceType": "waste",
  "errorType": "*errors.errorString"
}
```

### CLI Started
```json
{
  "anonymousId": "550e8400-e29b-41d4-a716-446655440000",
  "version": "2.0.0",
  "os": "darwin",
  "arch": "arm64",
  "event": "cli_started",
  "timestamp": "2025-10-22T14:45:00Z"
}
```

## Field Descriptions

### Common Fields (All Events)
| Field | Type | Description |
|-------|------|-------------|
| `anonymousId` | string | Randomly generated UUID stored in `~/.nebu/.anonymous_id` |
| `version` | string | CLI version (e.g., "2.0.0") |
| `os` | string | Operating system: `darwin`, `linux`, `windows` |
| `arch` | string | System architecture: `amd64`, `arm64`, `386` |
| `event` | string | Event type: `cli_started`, `scan_completed`, `scan_failed` |
| `timestamp` | string | ISO 8601 UTC timestamp (RFC3339 format) |

### Scan Events Only
| Field | Type | Description |
|-------|------|-------------|
| `durationMs` | number | Scan duration in milliseconds (optional) |
| `cloudProvider` | string | Cloud provider: `aws` (future: `gcp`, `azure`) |
| `serviceType` | string | Scan type: `waste`, `security`, `compliance`, `resilience` |
| `regionsCount` | number | Number of regions scanned (optional) |
| `findings` | object | Findings breakdown by service type (optional) |
| `errorType` | string | Go error type if scan failed (optional) |

### Findings Object Structure
```typescript
{
  "waste": {
    "critical": number,    // Critical business impact
    "high": number,        // High business impact
    "medium": number,      // Medium business impact
    "low": number,         // Low business impact
    "total": number,       // Total opportunities found
    "savings": number      // Estimated monthly savings in USD
  },
  "security": {
    "critical": number,    // Critical severity findings
    "high": number,        // High severity findings
    "medium": number,      // Medium severity findings
    "low": number,         // Low severity findings
    "total": number        // Total security findings
  }
}
```

## Privacy & Security

### What is Collected
- Anonymous installation UUID (not tied to any user identity)
- CLI version, OS, architecture
- Scan duration and regions count
- Aggregated counts of findings by severity
- Total estimated savings amount

### What is NOT Collected
- Cloud account IDs or credentials
- Resource names, IDs, ARNs, or any identifiers
- IP addresses or network information
- Personal data or PII
- Resource configurations or details
- Any data that could identify specific resources

## Backend Implementation Notes

### Expected HTTP Status Codes
- `200 OK` - Telemetry accepted
- `400 Bad Request` - Invalid payload format
- `429 Too Many Requests` - Rate limit exceeded
- `500 Internal Server Error` - Server error

### Recommended Rate Limiting
- Per `anonymousId`: 100 requests per hour
- Global: 10,000 requests per minute

### Database Schema Suggestions
```sql
CREATE TABLE telemetry_events (
  id BIGSERIAL PRIMARY KEY,
  anonymous_id UUID NOT NULL,
  event_type VARCHAR(50) NOT NULL,
  version VARCHAR(20) NOT NULL,
  os VARCHAR(20) NOT NULL,
  arch VARCHAR(20) NOT NULL,
  cloud_provider VARCHAR(20),
  service_type VARCHAR(50),
  duration_ms INTEGER,
  regions_count INTEGER,
  findings_critical INTEGER,
  findings_high INTEGER,
  findings_medium INTEGER,
  findings_low INTEGER,
  findings_total INTEGER,
  findings_savings DECIMAL(12,2),
  error_type VARCHAR(255),
  timestamp TIMESTAMPTZ NOT NULL,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_anonymous_id ON telemetry_events(anonymous_id);
CREATE INDEX idx_event_type ON telemetry_events(event_type);
CREATE INDEX idx_timestamp ON telemetry_events(timestamp);
CREATE INDEX idx_version ON telemetry_events(version);
```

## Usage Analytics Examples

### Daily Active Installations
```sql
SELECT DATE(timestamp) as date, COUNT(DISTINCT anonymous_id) as active_installations
FROM telemetry_events
WHERE event_type IN ('scan_completed', 'scan_failed')
GROUP BY DATE(timestamp)
ORDER BY date DESC;
```

### Average Scan Duration by Service Type
```sql
SELECT service_type, AVG(duration_ms/1000.0) as avg_duration_seconds
FROM telemetry_events
WHERE event_type = 'scan_completed' AND duration_ms IS NOT NULL
GROUP BY service_type;
```

### Total Savings Identified
```sql
SELECT SUM(findings_savings) as total_monthly_savings
FROM telemetry_events
WHERE event_type = 'scan_completed' AND service_type = 'waste';
```

### Scan Failure Rate
```sql
SELECT
  service_type,
  COUNT(CASE WHEN event_type = 'scan_failed' THEN 1 END)::float /
  COUNT(*)::float * 100 as failure_rate_percent
FROM telemetry_events
WHERE service_type IS NOT NULL
GROUP BY service_type;
```

## Testing

### Sample cURL Request
```bash
curl -X POST https://api.thenebu.com/api/cli/telemetry \
  -H "Content-Type: application/json" \
  -d '{
    "anonymousId": "550e8400-e29b-41d4-a716-446655440000",
    "version": "2.0.0",
    "os": "darwin",
    "arch": "amd64",
    "event": "scan_completed",
    "timestamp": "2025-10-22T14:30:00Z",
    "durationMs": 45000,
    "cloudProvider": "aws",
    "serviceType": "waste",
    "regionsCount": 17,
    "findings": {
      "waste": {
        "critical": 25,
        "high": 48,
        "medium": 156,
        "low": 89,
        "total": 318,
        "savings": 4250.50
      }
    }
  }'
```
