---
title: "Lambda và Service Catalog"
weight: 4
chapter: false
pre: " <b> 4. </b> "
---

## Mục đích

Phần này tạo lớp kết nối theo mẫu trong AWS blog:

- Lambda layer chứa Python dependency dùng để gọi GitHub APIs.
- Lambda function nhận EventBridge events từ SageMaker Model Registry.
- Service Catalog product hiển thị custom SageMaker project template trong Studio.

## Tạo Lambda layer

Trong sample repository, tìm thư mục Lambda layer. Package dependency layer theo hướng dẫn của repository.

Cấu trúc thường dùng:

```bash
mkdir -p lambda-layer/python
pip install requests -t lambda-layer/python
cd lambda-layer
zip -r github-actions-layer.zip python
cd ..
```

Publish layer:

```bash
aws lambda publish-layer-version \
  --layer-name github-actions-trigger-layer \
  --zip-file fileb://lambda-layer/github-actions-layer.zip \
  --compatible-runtimes python3.10 python3.11 \
  --region $AWS_REGION
```

Copy layer version ARN.

## Deploy Lambda trigger

Package và deploy Lambda function từ sample repository. Cấu hình environment variables:

| Variable | Description |
|---|---|
| `GITHUB_OWNER` | GitHub user hoặc organization |
| `GITHUB_REPO` | Workshop repository |
| `GITHUB_TOKEN_SECRET_NAME` | `github/personal-access-token` |
| `GITHUB_WORKFLOW_FILE` | Deployment workflow file, thường là `deploy.yml` |

Lambda execution role cần quyền đọc GitHub token secret và ghi CloudWatch logs.

## Tạo EventBridge rule

Tạo rule lắng nghe SageMaker Model Package state changes. Target là Lambda function. Rule này nối bước approve model với GitHub deployment.

## Publish Service Catalog product

![Service Catalog custom template](/images/mlops-sagemaker-github-actions/service-catalog.svg)

Dùng CloudFormation template trong sample repository để tạo Service Catalog product cho custom SageMaker project template.

Product nên nhận các parameters:

- GitHub owner.
- GitHub repository.
- GitHub branch.
- CodeConnections ARN.
- SageMaker project name.
- Model package group name.

Sau khi tạo product, associate product với portfolio được SageMaker Studio sử dụng.
