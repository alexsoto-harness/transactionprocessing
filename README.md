# Transaction Processing

Securely handles payment authorization, settlement, and reporting in real time using the payment gateway.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Architecture](#architecture)
- [Getting Started](#getting-started)
- [Configuration](#configuration)
- [Usage](#usage)
- [Testing](#testing)
- [Deployment](#deployment)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

**Transaction Processing** is a Java-based system designed for secure, real-time payment processing. It supports payment authorization, settlement, and comprehensive reporting, leveraging a robust payment gateway integration.

## Features

- Real-time payment authorization
- Secure transaction settlement
- Detailed reporting and transaction history
- Integration with payment gateway APIs
- Modular and extensible architecture
- Infrastructure as code (HCL/Terraform)
- Docker support for containerized deployment

## Architecture

The system is primarily written in Java, with infrastructure managed via HCL (Terraform) and containerized using Docker.


## Getting Started

### Prerequisites

- Java 11 or higher
- Maven or Gradle
- Docker (optional, for containerization)
- Terraform (optional, for infrastructure setup)

### Clone the Repository

```sh
git clone https://github.com/alexsoto-harness/transactionprocessing.git
cd transactionprocessing

# Run unit and integration tests
mvn test

# Or with Gradle
./gradlew test


# transactionprocessing

Welcome to your new Spring Boot application scaffolded using Cookiecutter! This project includes:

- A basic Spring Boot app (Java 17, Maven)
- A Dockerfile to containerize the app
- Terraform scripts to provision GKE (Google Kubernetes Engine)
- Kubernetes manifests to deploy your app
- A GitHub Actions CI/CD pipeline for build, deploy, and GKE rollout

---

## 🔧 Customizing Your App

### 1. Update the Java Package & Application Code

Your app starts with a simple controller located at:

```
app/src/main/java/com/transactionprocessing/HelloController.java
```

You can:
- Rename the package to reflect your organization (e.g., `com.myorg.myapp`)
- Add more endpoints and services as needed

If you change the package structure:
- Update `pom.xml` or `build.gradle` with the new `groupId`
- Move your files accordingly

---

### 2. Customize the Dockerfile

The `Dockerfile` assumes a Maven-built `.jar` located at:

```
target/transactionprocessing.jar
```

Ensure that the name of the JAR matches your build output, or modify the Dockerfile accordingly.

---

### 3. Update Kubernetes Manifests

Kubernetes YAML files are in the `k8s/` directory:

- `k8s/deployment.yaml`: change the container image name, labels, ports, and environment variables
- `k8s/service.yaml`: modify the service type and ports as needed

**Important:** Replace the image placeholder `gcr.io/YOUR_PROJECT_ID/transactionprocessing:latest` with your actual GCR or Artifact Registry path.

---

### 4. Configure Terraform for GKE

The Terraform config in `terraform/` provisions a GKE cluster:

- Update `terraform/variables.tf` with your actual `project_id`, `region`, and other variables
- If needed, modify `main.tf` to add VPC, IAM roles, or more complex infra

Run Terraform with:

```bash
cd terraform
terraform init
terraform plan
terraform apply
```

---

### 5. Configure GitHub Actions

The CI/CD pipeline is located at `.github/workflows/ci.yml`.

To make it work:
- Create GitHub Secrets:
  - `GCP_SA_KEY`: base64-encoded service account JSON with required GKE + Artifact Registry permissions
  - `GCP_PROJECT_ID`: your Google Cloud project ID
  - `GCP_REGION`: your desired region (e.g., `us-central1`)
  - `GKE_CLUSTER_NAME`: your GKE cluster name

The pipeline:
- Builds the Spring Boot app
- Builds and pushes a Docker image to GCR
- Deploys it to GKE using `kubectl`

You can extend it to include:
- Integration tests
- Canary rollouts
- Slack notifications, etc.
