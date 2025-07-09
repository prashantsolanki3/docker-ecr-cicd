# Multi-App Lambda CI/CD Project

This project demonstrates a CI/CD pipeline for deploying multiple AWS Lambda functions using Docker containers and GitHub Actions.

## Project Structure

```
cicd/
├── .github/workflows/
│   └── cicd.yml              # GitHub Actions CI/CD workflow
├── app1/
│   ├── lambda_function.py    # App1 Lambda handler
│   └── Dockerfile           # App1 container configuration
├── app2/
│   ├── lambda_function.py    # App2 Lambda handler
│   └── Dockerfile           # App2 container configuration
├── Dockerfile.lambda        # Base Lambda runtime image
└── README.md               # This file
```

## Applications

### App1
- **Lambda Function**: `MyApp1LambdaFunction`
- **ECR Repository**: `test_app1`
- **Response**: "Hello, World! - App1"

### App2
- **Lambda Function**: `MyApp2LambdaFunction`
- **ECR Repository**: `test_app2`
- **Response**: "Hello, World! - App2"

## CI/CD Workflow

The GitHub Actions workflow (`.github/workflows/cicd.yml`) performs the following steps:

1. **Base Image Build**: Creates a base Lambda runtime image
2. **App1 Deployment**: Builds and deploys App1 Lambda function
3. **App2 Deployment**: Builds and deploys App2 Lambda function

## Required Configuration Changes

### 1. GitHub Repository Settings

Update the following values in `.github/workflows/cicd.yml`:

```yaml
# Line 47: Update Lambda function name for App1
FUNCTION_NAME: MyApp1LambdaFunction  # Replace with your actual Lambda function name

# Line 48: Update ECR repository name for App1
IMAGE_REPO: test_app1               # Replace with your ECR repository name

# Line 95: Update Lambda function name for App2
FUNCTION_NAME: MyApp2LambdaFunction # Replace with your actual Lambda function name

# Line 96: Update ECR repository name for App2
IMAGE_REPO: test_app2               # Replace with your ECR repository name
```

### 2. GitHub Secrets

Add the following secrets to your GitHub repository:

1. Go to your repository → Settings → Secrets and variables → Actions
2. Add the following repository secrets:

| Secret Name | Description | Example Value |
|-------------|-------------|---------------|
| `AWS_ACCESS_KEY_ID` | AWS Access Key ID | `AKIAIOSFODNN7EXAMPLE` |
| `AWS_SECRET_ACCESS_KEY` | AWS Secret Access Key | `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY` |
| `AWS_REGION` | AWS Region | `us-east-1` |

### 3. AWS Resources Required

Before running the workflow, ensure you have:

1. **ECR Repositories**:
   - `test_base` (for base Lambda runtime image)
   - `test_app1` (for App1 Lambda function)
   - `test_app2` (for App2 Lambda function)

2. **Lambda Functions**:
   - `MyApp1LambdaFunction` (configured to use container images)
   - `MyApp2LambdaFunction` (configured to use container images)

3. **IAM Permissions**:
   - ECR push/pull permissions
   - Lambda update permissions

## Deployment Process

1. Push code to the `main` branch
2. GitHub Actions automatically triggers the workflow
3. Base image is built and pushed to ECR
4. App1 and App2 images are built and deployed to their respective Lambda functions

## Local Development

To test locally:

```bash
# Build base image
docker build -f Dockerfile.lambda -t lambda-base .

# Build App1
docker build -f app1/Dockerfile -t app1 app1/

# Build App2
docker build -f app2/Dockerfile -t app2 app2/
```

## Troubleshooting

- Ensure all AWS credentials are correctly configured
- Verify ECR repositories exist and are accessible
- Check that Lambda functions are configured to use container images
- Review GitHub Actions logs for detailed error messages 


AWS Policy

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "lambda:ListFunctions",
                "ecr:GetDownloadUrlForLayer",
                "lambda:UpdateFunctionConfiguration",
                "ecr:GetAuthorizationToken",
                "ecr:UploadLayerPart",
                "ecr:ListImages",
                "ecr:PutImage",
                "ecr:BatchGetImage",
                "ecr:CompleteLayerUpload",
                "ecr:DescribeImages",
                "ecr:DescribeRepositories",
                "ecr:InitiateLayerUpload",
                "ecr:BatchCheckLayerAvailability",
                "ecr:GetRepositoryPolicy"
            ],
            "Resource": "*"
        }
    ]
}
```