# SLI Throughput definition and ECS service deployment evaluation

## Status
Propposed [date: 01/18/26]

## Context
Define the expected traffic load for the Currency Exchange services, and evaluate deployment options to ensure scalability and reliability. AWS ECS is considered as a potential deployment option to handle variable workloads effectively.

AWS ECS Container orchestration benefits to consider:
- Independent deployable container units for API and UI services.
- Implement Fluentbit sidecar container to filter/create log chunks/process and forward logs to NewRelic (here some documentation). This would reduce Infrastructure costs, computation consumption (no aws agent processing logs), and this implementation would also improve times to visualize logs in NewRelic.
- Implement healthcheck mechanisms: 
    - Automatically trigger a new deployment if the system is unhealthy. The application team should expose a url path for /healthcheck, so ECS task can use it to monitor the application.
- Autoscaling capabilities based on CPU/Memory usage or custom CloudWatch metrics (e.g., request count, latency).

## Decision
1. Define expected throughput for API and UI services:
   - Estimate peak requests per second (RPS) based on anticipated user load.
   - Determine average and maximum response times to set performance benchmarks.

## Consequences
- If the expected throughput exceeds the capacity of a single EC2 instance, AWS ECS will be implemented to ensure scalability and reliability.