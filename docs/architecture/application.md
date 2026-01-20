# Reference Architecture Application

This section details the application layer components and their responsibilities within the Currency Exchange Rate platform. The architecture follows a microservice pattern with clear separation of concerns across three deployable components.

## Overview

The Reference Architecture Application is designed as a three-tier system optimized for reliability, observability, and scalability:

| Resources | Purpose |
| --- | --- |
| **AWS Lambda Service** | Updates RDS Aurora Database with required data | AWS SAM Deployment |
| **Data Layer** | Persists and retrieves currency exchange data from RDS Aurora |
| **UI Service*** | Provides the web interface for end users |
| **API Service** | Exposes REST endpoints for currency rate queries and operations |

### AWS Lambda service

**Responsibilities**:
- Poll external currency rate APIs on a scheduled interval
- Validate and transform exchange rate data
- Update RDS Aurora database with fresh rates
- Handle failures and retry logic for API calls
- Generate observability signals (logs, metrics, traces)

**Key Characteristics**:
- Generates correlation IDs for request tracing
- Implements structured logging to support observability standards
- Tracks execution duration and success/failure rates for SLO compliance

### Data Layer

**Purpose**: Persists and retrieves currency exchange rate data using RDS Aurora, serving as the central data store for both the API service and AWS Lambda service operations.

**Responsibilities**:
- Store current and historical exchange rates

**Key Characteristics**:
- Database schema enforces data integrity
- Implements data validation and transformation logic to ensure data quality

### Web UI and API Service

**Purpose**: Delivers a user-facing web interface for querying and displaying currency exchange rates based on a central business logic API layer, handling currency rate requests and coordinating data access.

**Responsibilities**:
- Render real-time currency rate displays (USD ↔ MXN)
- Provide user input interfaces for rate queries
- Integrate with the API service for data fetching
- Track user interactions and send observability signals

**Key Characteristics**:
- Docker containerized deployment for both UI and API service
- **UI service**: 
- Generates correlation IDs for request tracing
- Tracks request duration and status codes for SLO compliance
- Implements structured logging to support observability standards
- **API service**:
- Implements SLI-critical measurements (latency, availability, error rates)
- Tracks request duration and status codes for SLO compliance

### Deployment

- Both UI and API services run as independent Docker containers
- Stateless design enables multiple replicas behind load balancers
- Configuration management through environment variables and secrets management

### Observability Standards

Please refer to the [Observability Standards](../../README.md#observability-standards) section for detailed guidelines on logging, metrics, and tracing implementations across all application components to ensure consistent monitoring and alerting practices.