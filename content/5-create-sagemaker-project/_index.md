---
title: "Create SageMaker project"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

## Open SageMaker Studio

1. Open **Amazon SageMaker** in the AWS Console.
2. Open **Studio**.
3. Select your user profile.
4. Launch Studio.

## Create project from custom template

1. In Studio, open **Deployments** or **Projects**.
2. Choose **Create project**.
3. Select the custom template published through Service Catalog.
4. Enter project parameters:

| Parameter | Example |
|---|---|
| Project name | `github-actions-mlops-demo` |
| GitHub owner | Your GitHub user or organization |
| GitHub repository | `mlops-sagemaker-github-actions-workshop` |
| GitHub branch | `main` |
| CodeConnections ARN | ARN created earlier |
| Model package group | `github-actions-mlops-demo-model-group` |

5. Choose **Create project**.

## Review created resources

After provisioning finishes, validate:

- SageMaker project exists in Studio.
- Model package group exists in SageMaker Model Registry.
- GitHub repository contains workflow files.
- GitHub Actions has access to required secrets.
- Service Catalog provisioned product shows success.

## Trigger first build

Make a small commit or run workflow manually from GitHub Actions.

```bash
git commit --allow-empty -m "trigger first sagemaker pipeline run"
git push origin main
```

Open GitHub repository > **Actions** and watch the build workflow. The workflow should call AWS APIs and start SageMaker Pipeline.
