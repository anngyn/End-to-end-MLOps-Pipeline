---
title: "Chạy và kiểm tra pipeline"
weight: 6
chapter: false
pre: " <b> 6. </b> "
---

## Theo dõi GitHub Actions build

Mở repository trong GitHub, rồi chọn **Actions**. Build workflow cần:

1. Checkout source code.
2. Configure AWS credentials từ repository secrets.
3. Create hoặc update SageMaker Pipeline.
4. Start SageMaker Pipeline execution.

Nếu workflow lỗi, kiểm tra:

- Giá trị `AWS_REGION`.
- IAM user permissions.
- S3 bucket access.
- SageMaker execution role.
- Branch name và repository parameters.

## Theo dõi SageMaker Pipeline

![SageMaker pipeline approval](/images/mlops-sagemaker-github-actions/pipeline-approval.svg)

Trong SageMaker Studio:

1. Mở **Pipelines**.
2. Chọn pipeline do project tạo.
3. Mở execution mới nhất.
4. Kiểm tra từng step.

Các stage kỳ vọng:

- Processing hoặc data preparation.
- Training.
- Evaluation.
- Register model.

## Approve model

Mở **Model Registry** và chọn model package group do project tạo.

1. Mở model package mới nhất.
2. Kiểm tra metrics và artifacts.
3. Đổi approval status thành **Approved**.

Approval sẽ trigger EventBridge, sau đó Lambda, sau đó GitHub Actions deployment workflow.

## Kiểm tra deployment workflow

Quay lại GitHub **Actions** và mở deployment workflow run.

Workflow cần deploy hoặc update:

- Staging SageMaker endpoint.
- Production endpoint sau manual approval, tùy cấu hình workflow trong repository.

## Test endpoint

Dùng SageMaker runtime để invoke endpoint:

```bash
aws sagemaker-runtime invoke-endpoint \
  --endpoint-name <endpoint-name> \
  --content-type text/csv \
  --body fileb://sample-payload.csv \
  output.json \
  --region $AWS_REGION
cat output.json
```

## Troubleshooting

| Triệu chứng | Kiểm tra |
|---|---|
| GitHub workflow không dùng được credentials | Repository secrets và IAM permissions |
| Pipeline không được tạo | SageMaker execution role và workflow logs |
| Approve model không trigger deploy | EventBridge rule, Lambda logs, secret name |
| Lambda nhận event nhưng GitHub workflow không chạy | GitHub token permissions và workflow file name |
| Endpoint deployment lỗi | Instance quota, model artifact path, endpoint config logs |
