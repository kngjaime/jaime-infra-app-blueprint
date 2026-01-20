# Projects: Naming convention and Resource Tagging

## Naming Conventions for both Reference Architecture Infrastructure and Application Projects
Propposed general folder namming convention for Infrastructure and Appplication projects:

Pattern: `<Org>-<Div>-<BU>-<Env>-<Region>-<ResourceType>-<AppName>`

Org: Organization (e.g., lo)

Div: Division (e.g., fin for Finance)

BU: Business Unit (e.g., ps for Payment Suite)

Env: Environment (dev, stg, prd)

Region: Cloud Region (use1 for us-east-1, use2 for us-west-2)

RT: Resource Type Abbreviation (lmb for Lambda, rds for Database, s3b for S3 Bucket)

AppName: The specific PoC name (standard-poc)

Example for a PoC Lambda: lo-fin-ps-prd-use1-lmb-standard-poc

Note: There are shorten naming convention for some elements such as Environment (env), Region (reg), Resource Type (rt) to keep names concise. They should be always enfored in all projects' naming conventions and in the Infrastructure as Code (IaC) templates, so that way those resources can be easily identified and billed accordingly. 

#### Resource Tagging Strategy for Reference Architecture Infrastructure

All AWS resources should include the following tags to ensure proper identification, billing, and management:

`source_deployment_path`: The folder path where the IaC template is located

`provisioned_by`: Name or identifier of the person or team who provisioned the resource

`business_unit`: The business unit responsible for the resource (e.g., Payment Suite)

`environment`: The deployment environment (dev, stg, prd)

`project_name`: The name of the project (e.g., standard-poc)

`cloudformation_stack_arn`: The CloudFormation stack's ARN managing the resource

`cost_center`: The cost center code for billing purposes

`pipeline_name`: The name of the CI/CD pipeline used for deployments
