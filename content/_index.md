---
title: "End-to-End MLOps with SageMaker and GitHub Actions"
weight: 1
chapter: false
---

# End-to-End MLOps with SageMaker and GitHub Actions

This workshop converts the AWS Machine Learning Blog pattern into a hands-on lab. You will build a custom Amazon SageMaker project template that connects GitHub and GitHub Actions with SageMaker Pipelines, SageMaker Model Registry, staging deployment, and production approval.

![MLOps architecture](/images/mlops-sagemaker-github-actions/architecture.svg)

{{< mermaid >}}
flowchart LR
  Dev[Developer push] --> GH[GitHub repository]
  GH --> GHA[GitHub Actions build.yml]
  GHA --> SMP[SageMaker Pipeline]
  SMP --> Prep[Data preparation]
  Prep --> Train[Model training]
  Train --> Eval[Model evaluation]
  Eval --> Reg[Model Registry]
  Reg -->|Approve model| EB[EventBridge rule]
  EB --> L[Lambda trigger]
  L --> GHD[GitHub Actions deploy.yml]
  GHD --> STG[Staging endpoint]
  STG -->|Manual approval| PRD[Production endpoint]
{{< /mermaid >}}

## What you will build

- GitHub repository containing SageMaker pipeline and deployment code.
- AWS CodeConnections link from AWS to GitHub.
- Secrets Manager secret that stores a GitHub personal access token.
- IAM user and GitHub repository secrets used by GitHub Actions.
- Lambda function and Lambda layer used to trigger GitHub deployment workflow.
- Service Catalog product that exposes a custom SageMaker project template in Studio.
- SageMaker project that runs build and deploy workflows from GitHub Actions.

## Workshop sections

1. [Introduction](1-introduction)
2. [Prerequisites](2-preparatory-steps)
3. [GitHub and AWS setup](3-github-aws-setup)
4. [Lambda and Service Catalog](4-lambda-service-catalog)
5. [Create SageMaker project](5-create-sagemaker-project)
6. [Run and validate pipeline](6-run-validate-pipeline)
7. [Clean up resources](7-clean-up-resources)

## Source material

This workshop follows the AWS blog post "Build an end-to-end MLOps pipeline using Amazon SageMaker Pipelines, GitHub, and GitHub Actions" and the `aws-samples/mlops-sagemaker-github-actions` sample repository.
