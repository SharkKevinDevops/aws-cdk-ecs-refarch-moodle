# Moodle LMS on ECS Fargate

AWS CDK application to deploy highly available, elastic, and auto-scaling Moodle LMS using containers on AWS.

## Architecture

- **ECS Fargate**: Serverless Moodle containers
- **Application Load Balancer**: Load distribution and SSL termination
- **RDS Aurora**: Managed MySQL/PostgreSQL database
- **EFS**: Shared file storage for moodledata
- **CloudFront**: CDN for static assets
- **Auto Scaling**: Automatic container scaling

## Deployment Guide

### 1. Prerequisites

Install: AWS CLI, AWS CDK, Docker, Git

```bash
git clone https://github.com/aws-samples/aws-cdk-ecs-refarch-moodle.git
```

Request TLS certificate via AWS Certificate Manager (ACM) for your domain.

### 2. Build and Push Container Image

```bash
# Build Moodle image
docker build -t moodle-image src/image/src

# Create ECR repository and push
aws ecr create-repository --repository-name moodle-image --region [region]
docker tag moodle-image:latest [aws-account].dkr.ecr.[region].amazonaws.com/moodle-image:latest
docker push [aws-account].dkr.ecr.[region].amazonaws.com/moodle-image:latest
```

### 3. Deploy Infrastructure

Configure `cdk.json`:
- `albCertificateArn` and `cfCertificateArn`: ACM certificate ARNs
- `cfDomain`: CloudFront domain name
- `moodleImageUri`: Container image URI

```bash
npm install
cdk bootstrap
cdk deploy --all
```

### 4. Post-Deployment

- Create DNS CNAME record pointing to CloudFront/ALB endpoint
- Get Moodle credentials from CDK output (AWS Secrets Manager)
- Configure Redis cache using ElastiCache endpoint from CDK output
- Adjust replica count via `serviceReplicaDesiredCount` in `cdk.json`

### 5. Cleanup

```bash
cdk destroy --all
```

