# 3-Tier Web Application on AWS

## Architecture Overview
This project deploys a highly available 3-tier web application on AWS with the following components:

### Components
- **Web Tier**: Application Load Balancer + Auto Scaling Group of EC2 instances
- **Application Tier**: EC2 instances in private subnets
- **Database Tier**: RDS MySQL with Multi-AZ deployment
- **Networking**: VPC with public and private subnets across 2 AZs

### Security Features
- Security Groups with least privilege access
- Instances in private subnets with no public IP
- Database in isolated subnets

## Deployment
```bash
# Deploy the CloudFormation stack
aws cloudformation create-stack \
  --stack-name 3tier-web-app \
  --template-body file://infrastructure.yaml \
  --parameters ParameterKey=Environment,ParameterValue=dev \
  --capabilities CAPABILITY_NAMED_IAM
```

---
### Cost Estimation
- Estimated monthly cost: ~$50-80 (depending on usage)
- Uses t3.micro instances and db.t3.micro RDS

### Troubleshooting
Common issues and solutions:

1. Instance health checks failing: Check security group rules
2. Database connection issues: Verify VPC endpoints and security groups
