---
title: "Thiết lập GitHub và AWS"
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

## Tạo CodeConnections connection

1. Mở AWS Console.
2. Vào **Developer Tools** > **Settings** > **Connections**.
3. Chọn **Create connection**.
4. Provider: **GitHub**.
5. Connection name: `github-mlops-connection`.
6. Hoàn tất GitHub authorization và cài connector cho repository hoặc organization.
7. Copy connection ARN.

Đặt giá trị local:

```bash
export CODECONNECTION_ARN=<connection-arn>
```

## Tạo GitHub personal access token

Tạo GitHub fine-grained token hoặc classic token có quyền truy cập workshop repository. Token cần quyền trigger workflow dispatch events.

Lưu token vào Secrets Manager:

```bash
aws secretsmanager create-secret \
  --name github/personal-access-token \
  --secret-string '{"token":"<github-token>"}' \
  --region $AWS_REGION
```

## Tạo IAM user cho GitHub Actions

Tạo IAM user tên `github-actions-sagemaker-user`. Chỉ gán quyền cần thiết cho SageMaker Pipelines, SageMaker jobs, S3 artifacts, ECR nếu sample dùng, CloudFormation, và CloudWatch logs.

Với workshop account, bạn có thể bắt đầu từ policy trong sample repository, sau đó siết quyền lại sau lab.

Tạo access keys:

```bash
aws iam create-access-key --user-name github-actions-sagemaker-user
```

## Thêm GitHub repository secrets

![GitHub repository secrets](/images/mlops-sagemaker-github-actions/github-secrets.svg)

Trong GitHub, mở repository:

1. Vào **Settings** > **Secrets and variables** > **Actions**.
2. Thêm các repository secrets:

| Secret name | Value |
|---|---|
| `AWS_ACCESS_KEY_ID` | IAM access key ID |
| `AWS_SECRET_ACCESS_KEY` | IAM secret access key |
| `AWS_REGION` | Workshop Region, ví dụ `us-east-1` |
| `AWS_ACCOUNT_ID` | AWS account ID |

## Kiểm tra setup

Xác nhận access từ terminal:

```bash
aws sts get-caller-identity
aws codestar-connections list-connections --region $AWS_REGION
aws secretsmanager describe-secret --secret-id github/personal-access-token --region $AWS_REGION
```
