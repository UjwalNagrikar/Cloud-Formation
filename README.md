<div align="center">

# AWS CloudFormation Infrastructure Templates

<img src="https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg" alt="AWS" width="200"/>
<img src="https://raw.githubusercontent.com/yaml/yaml-spec/main/spec/yaml-logo.svg" alt="YAML" width="80" style="margin-left: 20px"/>

[![AWS](https://img.shields.io/badge/AWS-CloudFormation-orange?style=for-the-badge&logo=amazon-aws)](https://aws.amazon.com/cloudformation/)
![YAML Template](https://img.shields.io/badge/YAML-Template-blue?style=for-the-badge)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)
[![Region](https://img.shields.io/badge/Region-ap--south--1-yellow?style=for-the-badge)](https://aws.amazon.com/about-aws/global-infrastructure/regions_az/)

A collection of AWS CloudFormation templates for provisioning scalable and secure cloud infrastructure, featuring VPC networking and EC2 instances optimized for the Asia Pacific (Mumbai) region.

</div>

## 📋 Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Templates](#templates)
- [Prerequisites](#prerequisites)
- [Quick Start](#quick-start)
- [Deployment Options](#deployment-options)
- [Configuration](#configuration)
- [Security Considerations](#security-considerations)
- [Monitoring and Maintenance](#monitoring-and-maintenance)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## 🔍 Overview

This repository provides production-ready CloudFormation templates for deploying AWS infrastructure components. The templates are designed with best practices for security, scalability, and maintainability, specifically optimized for deployment in the `ap-south-1` (Asia Pacific - Mumbai) region.

### Key Features

- **Infrastructure as Code**: Declarative infrastructure management using CloudFormation
- **Modular Design**: Separate templates for different infrastructure patterns
- **Security-First**: Built-in security groups and network isolation
- **Cost-Optimized**: Uses cost-effective instance types and configurations
- **Regional Optimization**: Pre-configured for ap-south-1 region

## 🏗️ Architecture

### VPC Infrastructure (`aws-vpc.yaml`)

<div align="center">
<img src="https://d1.awsstatic.com/Products/product-name/diagrams/product-page-diagram_Amazon-VPC_HIW.b8086e18b32cac45c4a3c5e2ef82fd30e1cfc3a5.png" alt="AWS VPC Architecture" width="500"/>
</div>

```
┌─────────────────────────────────────────────────┐
│                    MyVPC                        │
│                 10.0.0.0/16                     │
│    🏷️  DNS Support & Hostnames Enabled          │
│                                                 │
│  ┌─────────────────────────────────────────────┐│
│  │             MySubnet                        ││
│  │            10.0.1.0/24                      ││
│  │          (ap-south-1a)                      ││
│  │    🌐 Auto-assign Public IP                 ││
│  │                                             ││
│  │  ┌─────────────────────────────────────────┐││
│  │  │    🖥️  EC2 Instance                     │││
│  │  │        (Amazon Linux 2)                 │││
│  │  │         t2.micro                        │││
│  │  │    🔒 Security Group (SSH:22)           │││
│  │  └─────────────────────────────────────────┘││
│  └─────────────────────────────────────────────┘│
│                     │                           │
│              ┌──────▼──────┐                    │
│              │  📋 Route   │                    │
│              │    Table    │                    │
│              └──────┬──────┘                    │
└─────────────────────┼─────────────────────────────┘
                      │
               ┌──────▼──────┐
               │  🌍 Internet │
               │   Gateway   │
               └─────────────┘
```

## 📄 Templates

<div align="center">

| Template | Purpose | Resources | Use Case |
|----------|---------|-----------|----------|
| 🏗️ **aws-vpc.yaml** | Complete Infrastructure | VPC + EC2 | Production Deployments |
| ⚡ **ec2.yaml** | Quick Instance | EC2 Only | Development & Testing |

</div>

### 1. Complete VPC Infrastructure (`aws-vpc.yaml`)

<img src="https://img.shields.io/badge/Resources-8-blue?style=flat-square"/> <img src="https://img.shields.io/badge/Complexity-Medium-orange?style=flat-square"/> <img src="https://img.shields.io/badge/Cost-Low-green?style=flat-square"/>

**Purpose**: Deploys a complete networking stack with EC2 instance

**Resources Created**:
- Custom VPC with DNS support
- Public subnet in ap-south-1a
- Internet Gateway with routing
- Security Group (SSH access)
- Amazon Linux 2 EC2 instance (t2.micro)

**Use Case**: Full infrastructure deployment for applications requiring custom networking

### 2. Standalone EC2 Instance (`ec2.yaml`)

<img src="https://img.shields.io/badge/Resources-1-blue?style=flat-square"/> <img src="https://img.shields.io/badge/Complexity-Simple-green?style=flat-square"/> <img src="https://img.shields.io/badge/Cost-Minimal-green?style=flat-square"/>

**Purpose**: Deploys a basic EC2 instance in default VPC

**Resources Created**:
- EC2 instance (t2.micro)
- Uses default VPC and security groups

**Use Case**: Quick instance deployment for development or testing

## 🛠️ Prerequisites

<div align="center">
<img src="https://img.shields.io/badge/AWS-CLI-orange?style=for-the-badge&logo=amazon-aws"/>
<img src="https://img.shields.io/badge/CloudFormation-IAM-blue?style=for-the-badge&logo=amazon-aws"/>
<img src="https://img.shields.io/badge/EC2-KeyPair-yellow?style=for-the-badge&logo=amazon-ec2"/>
</div>

Before deploying these templates, ensure you have:

### AWS Account Requirements
- Active AWS account with appropriate permissions
- IAM permissions for:
  - EC2 (Create, Describe, Terminate instances)
  - VPC (Create, Describe, Delete VPCs, Subnets, IGWs)
  - CloudFormation (Create, Update, Delete stacks)

### Tools and Configuration
- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) v2.x installed and configured
- Valid EC2 Key Pair created in `ap-south-1` region
- Basic understanding of AWS CloudFormation

### Verify Setup
```bash
# Verify AWS CLI configuration
aws sts get-caller-identity

# List available key pairs in ap-south-1
aws ec2 describe-key-pairs --region ap-south-1
```

## 🚀 Quick Start

<div align="center">
<img src="https://img.shields.io/badge/Deployment-Time-blue?style=flat-square"/> **~5-10 minutes**
</div>

### Deploy Complete VPC Infrastructure

1. **Clone the repository**:
   ```bash
   git clone <repository-url>
   cd aws-cloudformation-infrastructure
   ```

2. **Deploy the VPC stack**:
   ```bash
   aws cloudformation create-stack \
     --stack-name production-vpc-infrastructure \
     --template-body file://aws-vpc.yaml \
     --parameters ParameterKey=KeyName,ParameterValue=your-key-pair-name \
     --region ap-south-1 \
     --capabilities CAPABILITY_NAMED_IAM
   ```

3. **Monitor deployment**:
   ```bash
   aws cloudformation describe-stacks \
     --stack-name production-vpc-infrastructure \
     --region ap-south-1 \
     --query 'Stacks[0].StackStatus'
   ```

4. **Retrieve instance information**:
   ```bash
   aws cloudformation describe-stacks \
     --stack-name production-vpc-infrastructure \
     --region ap-south-1 \
     --query 'Stacks[0].Outputs'
   ```

### Deploy Standalone EC2 Instance

```bash
aws cloudformation create-stack \
  --stack-name development-ec2-instance \
  --template-body file://ec2.yaml \
  --parameters ParameterKey=KeyName,ParameterValue=your-key-pair-name \
  --region ap-south-1
```

## ⚙️ Configuration

### Template Parameters

#### VPC Template Parameters (`aws-vpc.yaml`)
| Parameter | Type | Required | Description | Default |
|-----------|------|----------|-------------|---------|
| KeyName | String | Yes | EC2 Key Pair name for SSH access | - |

#### EC2 Template Parameters (`ec2.yaml`)
| Parameter | Type | Required | Description | Default |
|-----------|------|----------|-------------|---------|
| KeyName | String | Yes | EC2 Key Pair name for SSH access | - |

### Customization Options

#### Modify Network Configuration
```yaml
# In aws-vpc.yaml, update CIDR blocks:
MyVPC:
  Properties:
    CidrBlock: 10.0.0.0/16  # Change to your preferred range

MySubnet:
  Properties:
    CidrBlock: 10.0.1.0/24  # Adjust subnet size
    AvailabilityZone: ap-south-1b  # Change AZ if needed
```

#### Update Instance Configuration
```yaml
# Modify instance specifications:
MyEC2Instance:
  Properties:
    InstanceType: t3.micro    # Upgrade instance type
    ImageId: ami-0abcdef123456789  # Use different AMI
```

## 🔒 Security Considerations

<div align="center">
<img src="https://img.shields.io/badge/Security-Grade-red?style=for-the-badge&logo=security"/>
<img src="https://img.shields.io/badge/Current-Basic-orange?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Recommended-Enhanced-green?style=for-the-badge"/>
</div>

### Current Security Configuration

- **SSH Access**: Currently allows SSH from anywhere (`0.0.0.0/0`)
- **Network Isolation**: VPC provides network-level isolation
- **Security Groups**: Configured for minimal required access

### Recommended Security Enhancements

1. **Restrict SSH Access**:
   ```yaml
   SecurityGroupIngress:
     - IpProtocol: tcp
       FromPort: 22
       ToPort: 22
       CidrIp: YOUR-OFFICE-IP/32  # Replace with your IP
   ```

2. **Add Additional Security Rules**:
   ```yaml
   # Add HTTPS access
   - IpProtocol: tcp
     FromPort: 443
     ToPort: 443
     CidrIp: 0.0.0.0/0
   
   # Add HTTP access
   - IpProtocol: tcp
     FromPort: 80
     ToPort: 80
     CidrIp: 0.0.0.0/0
   ```

3. **Enable VPC Flow Logs**:
   ```yaml
   VPCFlowLog:
     Type: AWS::EC2::FlowLog
     Properties:
       ResourceType: VPC
       ResourceId: !Ref MyVPC
       TrafficType: ALL
   ```

### Security Best Practices

- Always use the principle of least privilege
- Regularly rotate SSH keys
- Enable CloudTrail for audit logging
- Use Systems Manager Session Manager instead of SSH when possible
- Implement network ACLs for additional security layers

## 📊 Monitoring and Maintenance

<div align="center">
<img src="https://upload.wikimedia.org/wikipedia/commons/1/1d/AmazonCloudWatch.png" alt="CloudWatch" width="150"/>
</div>

### CloudFormation Stack Monitoring

```bash
# Check stack status
aws cloudformation describe-stacks --stack-name your-stack-name

# View stack events
aws cloudformation describe-stack-events --stack-name your-stack-name

# Monitor stack drift
aws cloudformation detect-stack-drift --stack-name your-stack-name
```

### EC2 Instance Monitoring

```bash
# Check instance status
aws ec2 describe-instances --instance-ids i-1234567890abcdef0

# View instance logs (requires CloudWatch agent)
aws logs describe-log-groups --log-group-name-prefix /aws/ec2
```

### Cost Monitoring

- Monitor costs using AWS Cost Explorer
- Set up billing alerts for unexpected charges
- Use AWS Trusted Advisor for cost optimization recommendations

## 🔧 Troubleshooting

### Common Issues and Solutions

#### Stack Creation Failures

**Issue**: Stack creation fails with "CREATE_FAILED" status
```bash
# Solution: Check stack events for detailed error
aws cloudformation describe-stack-events --stack-name your-stack-name
```

**Issue**: Key pair not found error
```bash
# Solution: Verify key pair exists in the correct region
aws ec2 describe-key-pairs --region ap-south-1
```

#### Instance Access Issues

**Issue**: Cannot SSH to instance
```bash
# Solution: Verify security group rules and instance state
aws ec2 describe-instances --instance-ids your-instance-id
aws ec2 describe-security-groups --group-ids your-security-group-id
```

**Issue**: Instance not receiving public IP
```bash
# Solution: Check subnet configuration
aws ec2 describe-subnets --subnet-ids your-subnet-id
```

### Debugging Commands

```bash
# Validate CloudFormation template
aws cloudformation validate-template --template-body file://aws-vpc.yaml

# Check resource limits
aws service-quotas get-service-quota --service-code ec2 --quota-code L-1216C47A

# View detailed error logs
aws cloudformation describe-stack-events \
  --stack-name your-stack-name \
  --query 'StackEvents[?ResourceStatus==`CREATE_FAILED`]'
```

## 🧹 Cleanup

<div align="center">
⚠️ **Important**: Always delete CloudFormation stacks to avoid unnecessary charges.

<img src="https://img.shields.io/badge/Cost-Alert-red?style=for-the-badge&logo=amazon-aws"/>
</div>

### Delete Resources

```bash
# Delete VPC stack (deletes all associated resources)
aws cloudformation delete-stack --stack-name production-vpc-infrastructure

# Delete standalone EC2 stack
aws cloudformation delete-stack --stack-name development-ec2-instance

# Verify deletion
aws cloudformation describe-stacks --stack-name your-stack-name
```

### Manual Cleanup (if needed)

If stack deletion fails, you may need to manually delete resources:

1. Terminate EC2 instances
2. Delete security groups
3. Detach and delete internet gateway
4. Delete subnets
5. Delete VPC

## 🤝 Contributing

We welcome contributions to improve these templates! Please follow these guidelines:

### How to Contribute

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Contribution Guidelines

- Follow AWS CloudFormation best practices
- Test all templates before submitting
- Update documentation for any changes
- Include comments for complex configurations
- Ensure templates are region-agnostic where possible

### Code Style

- Use consistent indentation (2 spaces)
- Include meaningful resource names
- Add comprehensive comments
- Follow CloudFormation naming conventions

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📞 Support

For support and questions:

- **Issues**: Create an issue in this repository
- **Documentation**: Check AWS CloudFormation documentation
- **Community**: AWS Developer Forums

## 🏷️ Version History

- **v1.0.0**: Initial release with VPC and standalone EC2 templates
  - Basic VPC infrastructure template
  - Standalone EC2 instance template
  - Documentation and deployment scripts

---

<div align="center">

**⚠️ Important Note**: These templates create billable AWS resources. Always monitor your AWS costs and delete resources when no longer needed.

<img src="https://img.shields.io/badge/Made%20with-❤️-red?style=for-the-badge"/>
<img src="https://img.shields.io/badge/AWS-CloudFormation-orange?style=for-the-badge&logo=amazon-aws"/>
<img src="https://img.shields.io/badge/Infrastructure-as--Code-blue?style=for-the-badge"/>

**Powered by AWS CloudFormation | Optimized for ap-south-1 Region**

</div>
