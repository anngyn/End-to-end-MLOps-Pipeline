---
title: "Lambda and Service Catalog"
weight: 4
chapter: false
pre: " <b> 4. </b> "
---

## Purpose

This section creates the glue layer used by the AWS blog pattern:

- Lambda layer contains the Python dependency used to call GitHub APIs.
- Lambda function receives EventBridge events from SageMaker Model Registry.
- Service Catalog product exposes the custom SageMaker project template in Studio.

## Create Lambda layer

From the sample repository, find the Lambda layer folder. Package the dependency layer as described by the repository instructions.

Typical shape:

```bash
mkdir -p lambda-layer/python
pip install requests -t lambda-layer/python
cd lambda-layer
zip -r github-actions-layer.zip python
cd ..
```

Publish the layer:

```bash
aws lambda publish-layer-version \
  --layer-name github-actions-trigger-layer \
  --zip-file fileb://lambda-layer/github-actions-layer.zip \
  --compatible-runtimes python3.10 python3.11 \
  --region $AWS_REGION
```

Copy the layer version ARN.

## Deploy Lambda trigger

Package and deploy the Lambda function from the sample repository. Configure environment variables:

| Variable | Description |
|---|---|
| `GITHUB_OWNER` | GitHub user or organization |
| `GITHUB_REPO` | Workshop repository |
| `GITHUB_TOKEN_SECRET_NAME` | `github/personal-access-token` |
| `GITHUB_WORKFLOW_FILE` | Deployment workflow file, usually `deploy.yml` |

The Lambda execution role needs permission to read the GitHub token secret and write CloudWatch logs.

## Create EventBridge rule

Create a rule that listens for SageMaker Model Package state changes. Target the Lambda function. This rule is what connects model approval to GitHub deployment.

## Publish Service Catalog product

![Service Catalog custom template](/images/mlops-sagemaker-github-actions/service-catalog.svg)

Use the CloudFormation template from the sample repository to create a Service Catalog product for the custom SageMaker project template.

The product should accept parameters such as:

- GitHub owner.
- GitHub repository.
- GitHub branch.
- CodeConnections ARN.
- SageMaker project name.
- Model package group name.

After product creation, associate it with the portfolio used by SageMaker Studio.
