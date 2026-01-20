# AMI, OSS and images security

## Status
Propposed [date: 01/18/26]

## Context
Two main security aspects are covered in this ADR, required to comply with high security standards:

1. Reference Infrastructure Architecture - Custom AMIs (host) 

- Create a custom pipeline to generate security-approved AMIs, you can build a lambda process to create it user-data input for initial configuration required, you can perform operations such , this once the EC2 instance is spun up. 

2. Reference Application Architecture -  Docker images (on the host)

- The organization should have an internal repository of Reference Docker Base mages and ensure OSS configures is approved by security team. The application teams should be able to leverage these References to implement their services, so that way the platform engineers would know what's the entrypoint to run an application on a given Infrastructure stack.


## Decision
1. Use a lambda or Hashicorp Packer to customize, generate and publish an AMI to different AWS accounts.

2. Install Docker and tool utilities such agents (appDynamics, NewRelic, Chef), create necessary filesystem structure to run a service with Reference Architecture standards.

3. Provide `user-data` customization to Provisioner Engineers, so that they can perform OS configurations for users creation/kernel configs/mount external EFS/install packages once an instance is spun up.

4. Configure `cloud-init` to initialize any process once and instance during instance's boot up stage.

5. TBD - For ECS deployments, use Bottlerocket (Container-Optimized OS) provides a minimal, immutable OS with automatic security updates, reduced attack surface through stripped-down components, and built-in security hardening that eliminates traditional OS vulnerabilities.  It offers optimized container runtime performance with direct integration to orchestrators like EKS/ECS, simplified node management through API-driven configuration, and faster boot times for rapid scaling operations.

## Consequences
Security-approved AMIs ensure compliance with organizational security policies and reduce vulnerabilities. Otherwise, using public AMIs may expose the infrastructure to security risks and potential breaches.