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

## ECS (Elastic Container Service)

Allows to launch Docker containers on AWS (e.g.: as EC2 instances), but you must provision and maintain the infrastructure (the EC2 instances). 
AWS takes care of starting / stopping containers. It has integration with Application Load Balancer (ALB).

## Fargate

Allows you to launch Docker containers **without the need to provision the infrastructure (no EC2 instances)**. It's a **serverless** offer.

## ECR (Elastic Container Registry)

It's a private docker registry on AWS so you can store you Docker images (to be later used in ECS or Fargate)

## API Gateway

Fully managed serverless service for developers to easily create, publish, maintain, monitor and secure APIs.
Supports RESTful APIs and WebScoket APIs and for security, user authentication, API throttling, API keys, monitoring...

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
