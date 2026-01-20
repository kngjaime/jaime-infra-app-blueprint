# SLO, SLI and Error Budgets

- SLI (Service Level Indicator): a measurable signal that indicates service behavior (for example, request success rate, latency, or availability).
- SLO (Service Level Objective): a target value or range for an SLI over a time window (for example, 99.9% successful requests over 30 days).
- Error budget: the allowed amount of SLO violation over the SLO evaluation window; it drives release and mitigation decisions.

## Proposed SLIs and Measurement Methods

### **Table 1: SLI Specifications and Measurement Methods**

| SLI | Specification | Measurement Method |
|-----|---------------|-------------------|
| Availability | Ratio of successful (2xx/3xx) HTTP responses to total requests. | `count(http_2xx) / count(total_requests)` |
| Latency | Time to serve an exchange-rate request (95th percentile). | API middleware `duration_ms` logs (p95) |
| Throughput | Number of requests handled per second (RPS) across the API cluster. | Load Balancer metrics (RequestCount / Duration) |
| Data Freshness | Age of the data served to the user (for a requested currency pair). | `now() - recorded_at` timestamp in MySQL |

⚠️ Important Notes:
- @HELP: Please review in detail points #2 and #3 from [Architecture Decision records](../../adr/architecture-decision-records.md) related to Throughput and Data Freshness SLIs. Further refinements may be necessary based on those decisions.

- All SLIs should explicitly reference the primary currency pair used by the product (USD → MXN) unless otherwise stated.


## SLO Targets and Windows

### **Table 2: 30 days cycle | SLO Targets**

| Objective | Target (SLO) | Error Budget |
|-----------|--------------|---------------------------|
| Availability | 99.9% | 43.2 minutes of downtime |
| Latency | 95% of requests must be < 300ms (p95) | Up to 5% of requests may exceed 300ms |
| Throughput | Minimum 100 RPS (operational requirement) | N/A (Capacity - used for scaling/alerts) |
| Data Freshness | Data age < 15 minutes for 99% of requests | ~7 hours of cumulative "stale data" time allowed |

### **Table 3: 30 days cycle vs 24 hours cycle | SLO Targets and Error Budgets**

| Objective | 30 days cycle | 24 hours cycle |
|-----------|---------------|----------------|
| Availability | 99.9% | 99.5% |
| Latency | 95% of requests must be < 300ms (p95) | 95% of requests must be < 400ms (p95) |
| Throughput | Minimum 100 RPS (operational requirement) | Minimum 80 RPS (operational requirement) |
| Freshness | Data age < 15 minutes for 99% of requests | Data age < 15 minutes for 95% of requests |

## Error Budget Policy and Actions

- Error budget calculation (clear form):
  - Error budget (fraction) = 1 - SLO_target. Example: SLO 99.9% → error_budget = 0.001 (0.1%).
  - Error consumed (fraction) = observed_error_rate over the SLO evaluation window.
  - Budget remaining (fraction) = error_budget - error_consumed.
  - To convert to time over a 30-day window: minutes_allowed = error_budget * (30 * 24 * 60).
  - Example: SLO 99.9% → error_budget = 0.001 → minutes_allowed = 0.001 * 43200 = 43.2 minutes. If observed error rate = 0.0005 (0.05%) → consumed = 21.6 minutes and budget_remaining = 21.6 minutes.

Policies (apply rolling windows unless otherwise stated):
- If >50% of the budget is consumed in the preceding rolling 7-day window: freeze risky releases, prioritize reliability work, and increase monitoring granularity.
- If >90% of the budget is consumed in the preceding rolling 7-day window: consider rolling back recent releases, escalate to on-call, and run a postmortem if impact thresholds are crossed.
- If budget is healthy: continue normal CI/CD cadence and feature rollouts with standard monitoring.

Responsibilities:
- On-call: immediate mitigation and runbook execution.
- Product/Engineering: prioritize reliability work to restore the budget and balance feature velocity with system reliability.

Reporting:
- Dashboard: display current SLI values, trends, remaining error budget (both fraction and converted time), and recent incidents.
- Weekly reliability report: include budget consumption, incidents, root causes, and actions taken.