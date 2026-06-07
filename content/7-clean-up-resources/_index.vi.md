---
title: "Dọn dẹp tài nguyên"
weight: 7
chapter: false
pre: " <b> 7. </b> "
---

## Xóa SageMaker endpoints

Xóa production và staging endpoints trước để dừng chi phí theo giờ:

```bash
aws sagemaker list-endpoints --region $AWS_REGION
aws sagemaker delete-endpoint --endpoint-name <endpoint-name> --region $AWS_REGION
aws sagemaker delete-endpoint-config --endpoint-config-name <endpoint-config-name> --region $AWS_REGION
```

## Xóa SageMaker project resources

Trong SageMaker Studio:

1. Mở **Projects**.
2. Chọn workshop project.
3. Xóa project resources nếu Studio có tùy chọn cleanup.

Trong CloudFormation, xóa stacks do SageMaker project và Service Catalog provisioned product tạo.

## Xóa Service Catalog resources

Xóa provisioned product, product, và portfolio association đã tạo cho custom template.

## Xóa Lambda và EventBridge

```bash
aws events list-rules --region $AWS_REGION
aws lambda list-functions --region $AWS_REGION
aws lambda delete-function --function-name <lambda-function-name> --region $AWS_REGION
```

Xóa Lambda layer versions nếu không còn dùng.

## Xóa secrets và IAM user

```bash
aws secretsmanager delete-secret \
  --secret-id github/personal-access-token \
  --force-delete-without-recovery \
  --region $AWS_REGION
```

Xóa IAM access keys, detach policies, và xóa `github-actions-sagemaker-user`.

## Xóa GitHub secrets

Trong GitHub repository settings, xóa:

- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `AWS_REGION`
- `AWS_ACCOUNT_ID`

## Kiểm tra cuối

Kiểm tra các nguồn chi phí còn sót:

- SageMaker endpoints.
- SageMaker training và processing jobs.
- CloudFormation stacks.
- S3 buckets và artifacts.
- Lambda functions và layers.
- Secrets Manager secrets.
- CodeConnections connection.
