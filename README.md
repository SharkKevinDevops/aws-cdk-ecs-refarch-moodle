# Moodle LMS on ECS Fargate

AWS CDK application to deploy highly available, elastic, and auto-scaling Moodle LMS using containers on AWS.

## Architecture

- **ECS Fargate**: Serverless Moodle containers
- **Application Load Balancer**: Load distribution and SSL termination
- **RDS Aurora**: Managed MySQL/PostgreSQL database
- **EFS**: Shared file storage for moodledata
- **CloudFront**: CDN for static assets
- **Auto Scaling**: Automatic container scaling

