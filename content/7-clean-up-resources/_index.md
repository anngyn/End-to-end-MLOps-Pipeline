---
title: "Clean up resources"
weight: 7
chapter: false
pre: " <b> 7. </b> "
---

## Delete SageMaker endpoints

Delete production and staging endpoints first to stop hourly charges:

```bash
aws sagemaker list-endpoints --region $AWS_REGION
aws sagemaker delete-endpoint --endpoint-name <endpoint-name> --region $AWS_REGION
aws sagemaker delete-endpoint-config --endpoint-config-name <endpoint-config-name> --region $AWS_REGION
```

## Delete SageMaker project resources

In SageMaker Studio:

1. Open **Projects**.
2. Select the workshop project.
3. Delete project resources if Studio exposes cleanup.

In CloudFormation, delete stacks created by the SageMaker project and Service Catalog provisioned product.

## Delete Service Catalog resources

Delete provisioned product, product, and portfolio association created for the custom template.

## Delete Lambda and EventBridge

```bash
aws events list-rules --region $AWS_REGION
aws lambda list-functions --region $AWS_REGION
aws lambda delete-function --function-name <lambda-function-name> --region $AWS_REGION
```

Delete Lambda layer versions if no longer needed.

## Delete secrets and IAM user

```bash
aws secretsmanager delete-secret \
  --secret-id github/personal-access-token \
  --force-delete-without-recovery \
  --region $AWS_REGION
```

Remove IAM access keys, detach policies, and delete `github-actions-sagemaker-user`.

## Delete GitHub secrets

In GitHub repository settings, delete:

- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `AWS_REGION`
- `AWS_ACCOUNT_ID`

## Final verification

Check for remaining cost sources:

- SageMaker endpoints.
- SageMaker training and processing jobs.
- CloudFormation stacks.
- S3 buckets and artifacts.
- Lambda functions and layers.
- Secrets Manager secrets.
- CodeConnections connection.
