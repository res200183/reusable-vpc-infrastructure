
# Reusable VPC Infrastructure — AWS CI/CD with CloudFormation

## Overview

This project demonstrates an automated Infrastructure as Code (IaC) deployment workflow using AWS CloudFormation, AWS CodePipeline, and AWS CodeBuild.

Infrastructure templates are stored in GitHub, validated automatically, and deployed to AWS through a CI/CD pipeline.

**Project Status: Successfully Deployed**

## Architecture

```text
GitHub Repository
       |
       v
AWS CodePipeline
       |
       +-- Source (GitHub)
       |
       +-- Build (AWS CodeBuild)
       |      |
       |      +-- AWS CLI Checks
       |      +-- CloudFormation Validation
       |      +-- BuildArtifact
       |
       +-- Deploy (AWS CloudFormation)
              |
              v
       ReusableEC2-Stack
              |
              v
       AWS Infrastructure
              |
              +-- VPC Networking
              +-- Amazon EC2
```

## Technologies Used

- AWS CloudFormation — Infrastructure as Code
- AWS CodePipeline — CI/CD automation
- AWS CodeBuild — Template validation
- Amazon EC2 — Compute infrastructure
- Amazon VPC — Networking
- AWS IAM — Roles and permissions
- Amazon S3 — Pipeline artifact storage
- AWS Systems Manager — Public AMI parameters
- GitHub — Version control

## Repository Structure

```text
reusable-vpc-infrastructure/
|
|-- buildspec.yml
|
|-- infrastructure/
|   |-- network.yaml
|   |-- ec2.yaml
```

## CI/CD Pipeline

The pipeline consists of three stages.

### 1. Source

AWS CodePipeline retrieves the source code from GitHub through a GitHub connection.

### 2. Build

AWS CodeBuild executes the commands defined in `buildspec.yml`.

Main tasks:

- Check AWS CLI availability
- Validate CloudFormation templates
- Prepare deployment artifacts
- Publish BuildArtifact

### 3. Deploy

AWS CodePipeline uses AWS CloudFormation to deploy the infrastructure.

CloudFormation creates or updates the stack:

`ReusableEC2-Stack`

The deployment process is automated.

## IAM Roles and Security

The project uses separate IAM service roles.

### CodePipeline Service Role

Responsible for coordinating pipeline execution and interacting with AWS services.

### CodeBuild Service Role

Responsible for executing build commands and validating infrastructure templates.

### CloudFormation Execution Role

`ReusableVPC-CloudFormationRole`

Responsible for creating and managing AWS infrastructure resources.

Required permissions include operations such as:

- cloudformation:CreateStack
- cloudformation:UpdateStack
- cloudformation:DescribeStacks
- cloudformation:DescribeStackEvents
- ec2:RunInstances
- ec2:DescribeImages
- ssm:GetParameters

IAM permissions must be restricted according to the principle of least privilege before production deployment.

## Deployment Process

1. Update the CloudFormation templates in GitHub.
2. Start the AWS CodePipeline execution.
3. CodePipeline retrieves the source code.
4. CodeBuild validates the infrastructure templates.
5. CloudFormation deploys the infrastructure.
6. CodePipeline reports the deployment result.

### Pipeline Execution

```text
Source  -> SUCCESS
Build   -> SUCCESS
Deploy  -> SUCCESS
```

## Troubleshooting Experience

During development, several AWS deployment issues were investigated and resolved.

### IAM AccessDenied

Identified missing IAM permissions by analyzing AWS error messages.

Examples:

- cloudformation:DescribeStacks
- cloudformation:CreateStack
- ssm:GetParameters
- ec2:DescribeImages
- ec2:RunInstances

### Missing Build Artifacts

Resolved deployment failures caused by missing CloudFormation templates in CodeBuild output artifacts.

### CloudFormation Rollback

Investigated failed stack deployments using CloudFormation Events.

Handled stacks in the `ROLLBACK_COMPLETE` state before retrying deployments.

## Key Learning Outcomes

This project provided practical experience with:

- Infrastructure as Code
- AWS CI/CD pipelines
- CloudFormation stack management
- IAM service roles and permissions
- AWS API authorization
- CodeBuild artifact configuration
- Deployment troubleshooting
- Automated infrastructure deployment

## Future Improvements

- Implement stricter IAM least-privilege policies
- Add CloudFormation linting
- Introduce multiple deployment environments
- Add infrastructure monitoring
- Improve automated security validation
- Implement automated infrastructure cleanup

## Project Status

**Completed: AWS CI/CD Pipeline Deployment**

All three pipeline stages — Source, Build, and Deploy — completed successfully.

