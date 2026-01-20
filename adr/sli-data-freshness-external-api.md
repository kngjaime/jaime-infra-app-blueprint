# SLI Data Freshness and External API service

## Status
Propposed [date: 01/19/26]

## Context

Frequent calls to external APIs pose significant risks due to API rate limiting and bot detection mechanisms implemented by most servers. This can cause service disruptions and blocked requests. We need to establish a strategy to ensure reliable data freshness while respecting API constraints.

### Options

1. **Upgrade to a Premium API Service**: Identify a more reliable API provider and obtain a paid API key. Premium services typically offer defined quotas and SLAs, with options to increase limits for additional cost.

2. **Implement Multi-Source Redundancy**: Establish at least 4-5 alternative data sources for critical information (e.g., USD exchange rates). This approach provides fallback options and reduces dependency on a single endpoint for real-time data requirements.

## Decision

1. Pursue a Premium API Service to ensure a stable and reliable data source with clear SLAs.

2. Define clear SLI and SLO targets for data freshness, incorporating error budgets to manage potential violations.

## Consequences

We could potentially impact costs by subscribing to a premium API service(s). However, this investment is justified by the need for reliability and data integrity. Implementing multi-source redundancy will enhance system resilience but may introduce additional complexity in data aggregation and validation processes.