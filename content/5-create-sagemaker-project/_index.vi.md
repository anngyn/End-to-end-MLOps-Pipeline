---
title: "Tạo SageMaker project"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

## Mở SageMaker Studio

1. Mở **Amazon SageMaker** trong AWS Console.
2. Mở **Studio**.
3. Chọn user profile.
4. Launch Studio.

## Tạo project từ custom template

1. Trong Studio, mở **Deployments** hoặc **Projects**.
2. Chọn **Create project**.
3. Chọn custom template đã publish qua Service Catalog.
4. Nhập project parameters:

| Parameter | Example |
|---|---|
| Project name | `github-actions-mlops-demo` |
| GitHub owner | GitHub user hoặc organization của bạn |
| GitHub repository | `mlops-sagemaker-github-actions-workshop` |
| GitHub branch | `main` |
| CodeConnections ARN | ARN đã tạo trước đó |
| Model package group | `github-actions-mlops-demo-model-group` |

5. Chọn **Create project**.

## Kiểm tra tài nguyên đã tạo

Sau khi provisioning hoàn tất, xác nhận:

- SageMaker project tồn tại trong Studio.
- Model package group tồn tại trong SageMaker Model Registry.
- GitHub repository có workflow files.
- GitHub Actions có đủ repository secrets.
- Service Catalog provisioned product thành công.

## Trigger build đầu tiên

Tạo commit nhỏ hoặc chạy workflow thủ công từ GitHub Actions.

```bash
git commit --allow-empty -m "trigger first sagemaker pipeline run"
git push origin main
```

Mở GitHub repository > **Actions** và theo dõi build workflow. Workflow cần gọi AWS APIs và start SageMaker Pipeline.
