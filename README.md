
---

# Strapi Deployment on AWS using ECS Fargate Spot with Terraform and CI/CD

## Project Overview

This project demonstrates deploying a containerized Strapi application on AWS using Amazon ECS with Fargate Spot. The infrastructure is provisioned using Terraform and deployment is automated through GitHub Actions.

The main objective of this task was to modify the existing ECS deployment to use Fargate Spot capacity and remove CloudWatch integration, as per internship requirements.

---

## Architecture Summary

The application follows a secure three-tier architecture:

Presentation Layer

* Application Load Balancer (ALB) deployed in Public Subnets
* Internet Gateway attached to the VPC

Application Layer

* Amazon ECS running Strapi container
* Uses Fargate Spot capacity provider
* Deployed in Private Subnets
* No public IP assigned

Data Layer

* Amazon RDS PostgreSQL
* Deployed in Private Subnets
* Public access disabled

Networking

* Custom VPC
* Public and Private Subnets
* NAT Gateway for outbound internet access
* Security Groups controlling traffic between layers

---

## Key Modification for This Task

The ECS service was updated to use:

capacity_provider_strategy with FARGATE_SPOT

The launch_type configuration was removed and replaced with:

FARGATE_SPOT capacity provider for cost optimization.


---

## Technologies Used

* AWS VPC
* Amazon ECS (Fargate Spot)
* Amazon RDS (PostgreSQL)
* Application Load Balancer
* Amazon ECR
* AWS IAM
* Terraform (Infrastructure as Code)
* GitHub Actions (CI/CD)
* Docker

---

## Deployment Workflow (CI/CD)

The deployment is automated using GitHub Actions:

1. Build Docker image
2. Push image to Amazon ECR
3. Run Terraform apply
4. Update ECS service
5. Wait for service stabilization

Docker images are tagged using commit SHA to ensure consistent deployments.

---

## Security Implementation

* Only ALB is publicly accessible
* ECS tasks do not have public IP addresses
* RDS is deployed in private subnets
* Security groups restrict communication between ALB, ECS, and RDS
* IAM roles used instead of hardcoded credentials

---

## Cost Optimization

This project uses Fargate Spot instead of standard Fargate to optimize compute cost.

Fargate Spot runs tasks on spare AWS capacity at a lower price. Tasks may be interrupted if AWS requires capacity, but this setup is suitable for development and internship tasks.

---

## Verification

After deployment, ECS tasks can be verified in the AWS Console under:

ECS → Cluster → Service → Tasks

The capacity provider shows:

FARGATE_SPOT

---

## Outcome

The Strapi application is successfully deployed using ECS Fargate Spot, connected securely to RDS, and accessible through the Application Load Balancer.

The infrastructure is fully provisioned using Terraform and deployment is automated through CI/CD.

---

This implementation fulfills the internship task requirement by:

* Migrating ECS to Fargate Spot
* Removing CloudWatch components
* Maintaining secure architecture
* Preserving automated deployment

---
