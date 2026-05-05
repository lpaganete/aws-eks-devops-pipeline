# AWS EKS DevOps Pipeline

A cloud-native CI/CD pipeline for deploying a web application on Amazon EKS using GitHub Actions, Docker, Terraform, Kubernetes, Prometheus, and Grafana.

## Overview

This project demonstrates a complete DevOps workflow for deploying a Flask-based web application and PostgreSQL database on AWS using Amazon EKS.

The infrastructure is provisioned with Terraform, while application build, push, and deployment steps are automated through GitHub Actions. The environment also includes observability with Prometheus and Grafana.

## Goals

- Automate infrastructure provisioning on AWS with Terraform.
- Build a reproducible CI/CD pipeline using GitHub Actions.
- Deploy a containerized application to a Kubernetes cluster on Amazon EKS.
- Expose the application with a Kubernetes LoadBalancer Service.
- Collect metrics and improve observability with Prometheus and Grafana.

## Architecture

The solution is composed of:

- **Backend and Database**
  - Flask web application.
  - PostgreSQL database deployed in Kubernetes.
- **Containerization**
  - Docker image built and pushed to Docker Hub.
- **Infrastructure**
  - AWS EKS cluster provisioned with Terraform.
- **CI/CD**
  - GitHub Actions workflow for build and deployment automation.
- **Observability**
  - Prometheus for metrics collection.
  - Grafana for dashboards and visualization.

## Tech Stack

- **Cloud:** AWS
- **Container Orchestration:** Kubernetes, Amazon EKS
- **CI/CD:** GitHub Actions
- **IaC:** Terraform
- **Containers:** Docker
- **Registry:** Docker Hub
- **Application:** Flask
- **Database:** PostgreSQL
- **Monitoring:** Prometheus, Grafana

## Repository Structure

```bash
.
├── kubernetes/
│   ├── postgres-deployment.yaml
│   ├── postgres-service.yaml
│   ├── app-deployment.yaml
│   └── app-service.yaml
├── terraform/
│   └── main.tf
├── .github/
│   └── workflows/
│       └── main.yaml
└── README.md
```

## Kubernetes Manifests

### PostgreSQL
- A Deployment manages the PostgreSQL pod.
- A Service exposes PostgreSQL on port `5432`.

### Fake Shop Application
- A Deployment runs 4 replicas of the Flask application.
- Prometheus annotations are added for metrics collection.
- A `LoadBalancer` Service exposes the application externally.

## CI/CD Pipeline

The GitHub Actions workflow handles both CI and CD:

### CI Stage
- Checks out the source code.
- Builds the Docker image.
- Pushes the image to Docker Hub.

### CD Stage
- Configures access to the EKS cluster with `kubectl`.
- Applies the Kubernetes manifests.
- Deploys the latest application image to the cluster.

## Infrastructure as Code

Terraform is used to provision the AWS infrastructure required for the project, including the EKS cluster.

### Main Terraform File
- `main.tf`: Defines the AWS provider, networking configuration, and EKS cluster resources.

### Deployment Steps
```bash
terraform init
terraform plan
terraform apply
```

## Accessing the Cluster

After provisioning, update your kubeconfig:

```bash
aws eks update-kubeconfig --region <your-region> --name <cluster-name>
```

Then deploy the manifests:

```bash
kubectl apply -f <path-to-manifests>
```

## Configuration

### Prerequisites

#### Docker Hub Secrets
Configure the following secrets in GitHub:

- `DOCKERHUB_USERNAME`
- `DOCKERHUB_TOKEN`

#### AWS Secrets
Configure the following secrets in GitHub:

- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `AWS_SECRET_REGION`

### GitHub Actions
Make sure repository actions permissions are enabled to allow CI/CD execution.

## Deployment on AWS

To trigger the pipeline:

```bash
git add .
git commit -m "Update project"
git push origin main
```

Then monitor the workflow execution in GitHub Actions.

## Future Improvements

- Add a message/event logging system.
- Create more custom Grafana dashboards.
- Add ingress with HTTPS using AWS Load Balancer Controller or NGINX Ingress.
- Add autoscaling for application and cluster workloads.

## What This Project Demonstrates

This project highlights practical experience with:

- AWS and EKS
- Terraform-based infrastructure provisioning
- GitHub Actions CI/CD pipelines
- Kubernetes deployments and services
- Docker image automation
- Monitoring and observability with Prometheus and Grafana

## Notes

This repository was built as a DevOps bootcamp project and later refined as part of a cloud and DevOps portfolio.