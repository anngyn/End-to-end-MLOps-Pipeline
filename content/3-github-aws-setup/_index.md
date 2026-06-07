---
title: "GitHub and AWS setup"
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

## Create CodeConnections connection

1. Open the AWS Console.
2. Go to **Developer Tools** > **Settings** > **Connections**.
3. Choose **Create connection**.
4. Provider: **GitHub**.
5. Connection name: `github-mlops-connection`.
6. Complete GitHub authorization and install the connector on your repository or organization.
7. Copy the connection ARN.

Set the value locally:

```bash
export CODECONNECTION_ARN=<connection-arn>
```

## Create GitHub personal access token

Create a GitHub fine-grained token or classic token with access to the workshop repository. It must be able to trigger workflow dispatch events.

Save the token in Secrets Manager:

```bash
aws secretsmanager create-secret \
  --name github/personal-access-token \
  --secret-string '{"token":"<github-token>"}' \
  --region $AWS_REGION
```

## Create IAM user for GitHub Actions

Create an IAM user named `github-actions-sagemaker-user`. Attach only permissions needed for SageMaker Pipelines, SageMaker jobs, S3 artifacts, ECR if used by the sample, CloudFormation, and CloudWatch logs.

For a workshop account, you can start with the policy provided by the sample repository, then tighten it after the lab.

Create access keys:

```bash
aws iam create-access-key --user-name github-actions-sagemaker-user
```

## Add GitHub repository secrets

![GitHub repository secrets](/images/mlops-sagemaker-github-actions/github-secrets.svg)

In GitHub, open your repository:

1. Go to **Settings** > **Secrets and variables** > **Actions**.
2. Add these repository secrets:

| Secret name | Value |
|---|---|
| `AWS_ACCESS_KEY_ID` | IAM access key ID |
| `AWS_SECRET_ACCESS_KEY` | IAM secret access key |
| `AWS_REGION` | Workshop Region, for example `us-east-1` |
| `AWS_ACCOUNT_ID` | AWS account ID |

## Validate setup

Confirm access from your terminal:

```bash
aws sts get-caller-identity
aws codestar-connections list-connections --region $AWS_REGION
aws secretsmanager describe-secret --secret-id github/personal-access-token --region $AWS_REGION
```
