---
title: "Run and validate pipeline"
weight: 6
chapter: false
pre: " <b> 6. </b> "
---

## Monitor GitHub Actions build

Open your repository in GitHub, then choose **Actions**. The build workflow should:

1. Check out source code.
2. Configure AWS credentials from repository secrets.
3. Create or update SageMaker Pipeline.
4. Start SageMaker Pipeline execution.

If the workflow fails, check:

- `AWS_REGION` value.
- IAM user permissions.
- S3 bucket access.
- SageMaker execution role.
- Branch name and repository parameters.

## Monitor SageMaker Pipeline

![SageMaker pipeline approval](/images/mlops-sagemaker-github-actions/pipeline-approval.svg)

In SageMaker Studio:

1. Open **Pipelines**.
2. Select the pipeline created by the project.
3. Open the latest execution.
4. Validate each step.

Expected stages:

- Processing or data preparation.
- Training.
- Evaluation.
- Register model.

## Approve model

Open **Model Registry** and select the model package group created by the project.

1. Open the latest model package.
2. Review metrics and artifacts.
3. Change approval status to **Approved**.

Approval triggers EventBridge, then Lambda, then GitHub Actions deployment workflow.

## Validate deployment workflow

Return to GitHub **Actions** and open the deployment workflow run.

The workflow should deploy or update:

- Staging SageMaker endpoint.
- Production endpoint after manual approval, depending on repository workflow configuration.

## Test endpoint

Use SageMaker runtime to invoke the endpoint:

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

| Symptom | Check |
|---|---|
| GitHub workflow cannot assume or use credentials | Repository secrets and IAM permissions |
| Pipeline not created | SageMaker execution role and workflow logs |
| Model approval does not trigger deploy | EventBridge rule, Lambda logs, secret name |
| Lambda receives event but GitHub workflow does not start | GitHub token permissions and workflow file name |
| Endpoint deployment fails | Instance quota, model artifact path, endpoint config logs |
