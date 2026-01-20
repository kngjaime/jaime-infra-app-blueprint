# Reference Architecture Infrastructure

This section describes the cloud infrastructure components, deployment strategies, and operational considerations for the Currency Exchange Rate platform. The infrastructure is designed to be highly available, scalable, and production-ready.

## Pre-requirement: AWS VPC and subnets

Highlevel information to provision VPC and subnets. These resources will be provisioned using CloudFormation configuration files which should follow high standards.

**Network Security**:
- VPC with public and private subnets with CIDR block allocation
- Network ACLs and security groups enforce least privilege (not required)
- All inter-service communication over private networks
- VPC Flow Logs for network traffic monitoring for best Security practices
- Subnets configured for application tiers (public for ALB, private for services, secure for databases)

**Gateways Networking**:
- Internet Gateway attached to the VPC
- NAT Gateway in public subnets for outbound internet access from private subnets
- Route Tables configured for public and private subnets


## Overview

This document describes required Infrastructure resoruces to support the 4 main components in [Architecture Overview](../../README.md#architecture-overview) section for the Exchange Currency Rate project.


Required Infrastructure resources:

| Resources | Purpose |
| --- | --- |
| **AWS Lambda** | Scheduled data updates to RDS Aurora |
| **RDS Aurora** | Persistent storage for Application |
| **EC2 or ECS service (TBD)** | Host for Application|
| **Application Load Balancer (ALB)** | Traffic distribution and routing |
| **EventBridge triggers** | Scheduled event triggering |
| **ECR** | Container image registry |
| **Route 53** | DNS and domain management |


### AWS Lambda Service

**Infraestructure Deployment**:
- AWS Serverless Application Model (SAM) for Infrastructure-as-Code
- Lambda Environment variables to support multi environment deployments.
- EventBridge triggers for scheduled execution
- Environment-based configuration for API credentials and endpoints

**Reliability Features**:
- Scheduled execution window with configurable frequency
- Retry mechanisms for external API failures
- CloudWatch alarms for execution errors, max concurrent executions, duration threshold, throttling. Each one of the alarms should allow custom values to define maximum allowed values in a scale range from  Warning, High and Critical. 

### RDS Aurora Database

**Infraestructure Deployment**:
- AWS Serverless Application Model (SAM) for Infrastructure-as-Code

**Performance**:
- Connection pooling configured at application layer
- Query optimization and indexing on exchange rate lookup columns
- CloudWatch monitoring dashboard for database metrics (CPU, connections, replication lag)

### API and UI Services Infrastructure

**Deployment**:
- Docker containerized application
- Container registry (ECR - Elastic Container Registry) for image storage
- TBD EC2 vs EC2
- Application Load Balancer (ALB) for traffic distribution with TLS 1.2 and custom R53 domain pointing to ALB/

**Scaling**:
TBD


### Monitoring & Observability Infrastructure

**CloudWatch**:
- Centralized log aggregation from all services
- Custom metrics for business KPIs and SLIs
- Dashboards for real-time system health visualization
- Alarms configured for SLO violations and anomalies

**Distributed Tracing**:
- AWS X-Ray for end-to-end request tracing
- Service map visualization for dependency analysis

##### Infrastructure as Code (IaC)

**Tools**:
- AWS SAM for Lambda deployments
- CloudFormation comprehensive infrastructure
- Docker Compose for local development and Application deployment.
- Environment-specific configuration files (dev, stg, prd)

### Observability Standards

Please refer to the [Observability Standards](../../README.md#observability-standards) section for detailed guidelines on logging, metrics, and tracing implementations across all application components to ensure consistent monitoring and alerting practices.
