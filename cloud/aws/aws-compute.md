# AWS Compute

## Summary 
- Docker: container technology to run applications
- ECS: run Docker containers on EC2 instances
- Fargate:
  - Run Docker containers without provisioning the infrastructure
  - Serverless offering (no EC2 instances)
- ECR: Private Docker Images Repository
- Batch: run batch jobs on AWS across managed EC2 instances
- Lightsail: predictable & low pricing for simple application & DB stacks

## ECS

## ECR

## API Gateway

## Lambda

## AWS Batch

### Batch vs Lamnda

| Lambda | Batch |
| ------ | ----- |
| Time Limit | No Time Limit |
| Limited runtimes | Any runtime so long its packaged as a Docker Image |
| Limited temporary disk space | Rely on EBS / instance store for disk space |
| Serverless | Relies on EC2 (can be managed by AWS) |

## Lightsail
