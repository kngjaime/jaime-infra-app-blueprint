#### Unified Logging Schema (JSON)

Enforce a JSON schema across your services (UI, API, Lambda). This allows tools like Datadog, ELK, or CloudWatch Insights to parse, index, and visualize your data to gain insights about the services behaviour.

Each service component integrates observability from the ground up:

- **Correlation IDs**: Trace requests across UI → API → Database
- **Structured Logging**: All services emit JSON logs with standardized schema
- **Metrics**: Record latency (p95, p99), availability (status codes), and custom business metrics
- **Health Checks**: Implement liveness and readiness probes for Kubernetes/container orchestration

<details>

<summary> Logging proposal format sample </summary>

```{
  "timestamp": "2026-01-18T10:00:00.123Z",
  "level": "INFO", 
  "service": "currency-api",           // The specific microservice
  "correlation_id": "abc-123-xyz",     // UNIQUE per request (Trace ID)
  "conversation_id": "session-999",    // UNIQUE per user session
  "user_id": "u_555",                  // (Optional) Anonymized User ID
  
  // CONTEXT: The specific data for this event
  "event": "rate_fetch_success",       // standardized event name
  "meta": {
    "currency_pair": "USD-MXN",
    "provider": "OpenExchange",
    "duration_ms": 145,                // CRITICAL for Latency SLI
    "status_code": 200,                // CRITICAL for Availability SLI
    "db_query_time_ms": 12,
    "cache_hit": true
  },
  
  // ERROR DETAILS (Only present on error)
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Upstream provider rejected request",
    "stack_trace": "..."
  }
}
```
</details>