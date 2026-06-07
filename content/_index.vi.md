---
title: "MLOps end-to-end với SageMaker và GitHub Actions"
weight: 1
chapter: false
---

# MLOps end-to-end với SageMaker và GitHub Actions

Workshop này chuyển nội dung trong AWS Machine Learning Blog thành bài lab thực hành. Bạn sẽ tạo custom Amazon SageMaker project template để kết nối GitHub và GitHub Actions với SageMaker Pipelines, SageMaker Model Registry, staging deployment, và bước duyệt production.

![Kiến trúc MLOps](/images/mlops-sagemaker-github-actions/architecture.svg)

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

## Bạn sẽ xây gì

- GitHub repository chứa code SageMaker pipeline và deployment.
- AWS CodeConnections kết nối AWS với GitHub.
- Secrets Manager secret lưu GitHub personal access token.
- IAM user và GitHub repository secrets cho GitHub Actions.
- Lambda function và Lambda layer để trigger GitHub deployment workflow.
- Service Catalog product hiển thị custom SageMaker project template trong Studio.
- SageMaker project chạy build và deploy workflow từ GitHub Actions.

## Nội dung workshop

1. [Giới thiệu](1-introduction)
2. [Chuẩn bị](2-preparatory-steps)
3. [Thiết lập GitHub và AWS](3-github-aws-setup)
4. [Lambda và Service Catalog](4-lambda-service-catalog)
5. [Tạo SageMaker project](5-create-sagemaker-project)
6. [Chạy và kiểm tra pipeline](6-run-validate-pipeline)
7. [Dọn dẹp tài nguyên](7-clean-up-resources)

## Nguồn tham khảo

Workshop bám theo AWS blog "Build an end-to-end MLOps pipeline using Amazon SageMaker Pipelines, GitHub, and GitHub Actions" và sample repository `aws-samples/mlops-sagemaker-github-actions`.
