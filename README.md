#  PoC: EXCHANGE CURRENCY RATE PROJECT

##  Table of Contents
- [Objective and Scope](#objective-and-scope)
- [Architecture Overview](#architecture-overview)
  - [Reference Architecture Infrastructure](#reference-architecture-infrastructure)
  - [Reference Architecture Application](#WIP-reference-architecture-application)
  - [Highlevel Architecture Diagrams](#highlevel-architecture-diagrams)
  - [Observability & Standards](#observability-standards)
    - [Projects: Naming convention and Resource Tagging](#projects-naming-convention-and-resource-tagging)
    -  [Unified Logging Schema (JSON)](#unified-logging-schema-json)
    - (TBD)[Observability Integration](#tbdobservability-integration)
- [reliability Policies](#reliability-policies)
  - [Reliability Policies](#reliability-policies)
    - [SLO, SLI and Error Budgets](#slo-sli-and-error-budgets)
    - [(TBD) DNS/Regional Outages](#tbddnsregional-outages)
- [Architecture Decision Records](#architecture-decision-records)

##  Objective and Scope

- Core Objective: Build a Framework and System to define and align Production Readiness Review (PRR) standards. The primary goal is not just to create a PoC Porject for Currency Exchange Platform, but to create a **Reference Architecture** that serves as a Blueprint to define how reliable, scalable, and observable services should be built. This PoC will demonstrate the practical implementation of these standards across Infrastructure and Application layers.

- The Proof of Concept (PoC): A High-Availability Currency Exchange Platform (USD $\leftrightarrow$ MXN). This application will serve as the "Reference Architecture Implementation."

## Architecture Overview

The architecture emphasizes security, observability, scalability, and reliability while maintaining clear separation between the the 3 main Application layers: Data, API and UI layerss.

The system consists of 4 main application components, each one will have independent deployable configurations units based on the service type.


| Web service resources | Description | Infraestructure Deployment type | Application Deployment type |
| --- | --- | --- | --- |
| 1. **AWS RDS (database service)** | RDS Aurora Database with schema definition | AWS SAM Deployment | SQL Schema Definition
| 2. **AWS Lambda (serverless service)** | Updates RDS Aurora Database with required data | AWS SAM Deployment | S3 artifact zip file |
| 3. **API (microservice-based)** | Reads data from a RDS Aurora Database | EC2 Cloudformation stack  | Docker containerized app |
| 4. **UI (microservice-based)** | Provides a web interface for users.  | EC2 Cloudformation stack | Docker containerized app |

**Important note:** Plese review the following Architecture Decision record [SLI: Throughput definition and ECS service deployment evaluation](adr/sli-throughput-ecs-evaluation.md). This is requied to implement application components #3 and #4. 

The services are designed to meet strict service quality standards with production-ready configurations.

### [Reference Architecture Infrastructure](docs/architecture/infrastructure.md)


### [Reference Architecture Application](docs/architecture/application.md)


### [Highlevel Architecture Diagrams](docs/architecture/diagrams.md)

##  Observability Standards

A clear Naming Convention and Tagging Strategy aligns DevOps, SRE, and Finance (FinOps) teams, ensuring resources are searchable, billable, and manageable at scale. Standardized logging enables consistent processing, ingestion, and monitoring of multi micro services interactions and their impact on system traffic.

### [Projects: Naming convention and Resource Tagging](docs/standards/naming-and-tagging.md)

### [Unified Logging Schema (JSON)](docs/standards/service-logging.md)

### (TBD)[Observability Integration]

TBD - Integration with Observability tools such as Datadog, NewRelic, Splunk, etc.

## Reliability Policies

Defining clear Reliability Policies is essential to complement the Software Standardization process and support the entire Software Development Lifecycle. This ensures alignment with the Production Readiness Review (PRR) standards established in this Reference Architecture.

Reliability Policies enable cross-team collaboration across multiple areas, including Developers, PMs, Change Advisory Board (CAB), CX, SRE, DevOps, and Support teams. By establishing measurable reliability targets (SLOs/SLIs) and error budgets, these policies provide a shared framework for:

- **Risk Management**: Balancing feature velocity with system stability
- **Incident Response**: Clear escalation paths and remediation procedures
- **Resource Allocation**: Justifying investment in reliability engineering
- **Accountability**: Establishing service quality expectations across teams


### [SLO, SLI and Error Budgets](docs/reliability/slo-sli-error-budgets.md)

### (TBD)DNS/Regional Outages

## Architecture Decision records

Please find the list of Architecture Decision Records (ADRs) documenting key decisions which will require to be addressed once this Software Design Document is reviewed and approved for this Reference Architecture. 

### [List of Architecture Decision records](adr/architecture-decision-records.md)
