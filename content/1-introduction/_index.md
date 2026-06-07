---
title: "Introduction"
weight: 1
chapter: false
pre: " <b> 1. </b> "
---

## Overview

In this workshop, you build an MLOps pipeline that uses GitHub as the source code repository and GitHub Actions as the CI/CD engine. SageMaker Pipelines handles ML workflow orchestration, and SageMaker Model Registry controls model promotion.

The pattern has two main automation paths:

- Source code changes trigger a GitHub Actions build workflow.
- Model approval in SageMaker Model Registry triggers a deployment workflow through EventBridge and Lambda.

## Architecture

{{< mermaid >}}
flowchart TD
  A[GitHub source repository] --> B[GitHub Actions build workflow]
  B --> C[SageMaker Pipeline]
  C --> D[Training job]
  D --> E[Evaluation step]
  E --> F[Model package group]
  F --> G{Model approved?}
  G -->|Yes| H[EventBridge]
  H --> I[Lambda]
  I --> J[GitHub Actions deploy workflow]
  J --> K[Staging endpoint]
  K --> L[Production approval]
  L --> M[Production endpoint]
{{< /mermaid >}}

## Learning objectives

After finishing this workshop, you can:

- Connect AWS to GitHub with CodeConnections.
- Store a GitHub token in Secrets Manager for automation.
- Configure GitHub Actions secrets for AWS access.
- Deploy a Lambda function that starts GitHub Actions workflow runs.
- Publish a custom SageMaker project template with Service Catalog.
- Create a SageMaker project from Studio.
- Run SageMaker Pipeline and promote a model from staging to production.

## Estimated time and cost

Plan for 90-120 minutes. Cost comes mainly from SageMaker Studio, SageMaker jobs, endpoints, S3, Lambda, and CloudWatch logs. Delete endpoints and stacks at the end.
