---
title: "Giới thiệu"
weight: 1
chapter: false
pre: " <b> 1. </b> "
---

## Tổng quan

Trong workshop này, bạn xây MLOps pipeline dùng GitHub làm source repository và GitHub Actions làm CI/CD engine. SageMaker Pipelines điều phối ML workflow, SageMaker Model Registry kiểm soát việc promote model.

Mẫu kiến trúc có hai luồng tự động hóa chính:

- Source code thay đổi sẽ trigger GitHub Actions build workflow.
- Model được approve trong SageMaker Model Registry sẽ trigger deployment workflow thông qua EventBridge và Lambda.

## Kiến trúc

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

## Mục tiêu học tập

Sau workshop, bạn có thể:

- Kết nối AWS với GitHub bằng CodeConnections.
- Lưu GitHub token trong Secrets Manager cho automation.
- Cấu hình GitHub Actions secrets để truy cập AWS.
- Deploy Lambda function dùng để khởi chạy GitHub Actions workflow run.
- Publish custom SageMaker project template bằng Service Catalog.
- Tạo SageMaker project từ Studio.
- Chạy SageMaker Pipeline và promote model từ staging sang production.

## Thời gian và chi phí

Dự kiến 90-120 phút. Chi phí chủ yếu đến từ SageMaker Studio, SageMaker jobs, endpoints, S3, Lambda, và CloudWatch logs. Hãy xóa endpoints và stacks ở cuối lab.
