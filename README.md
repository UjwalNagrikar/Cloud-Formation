# AWS VPC & EC2 CloudFormation Template

This repository contains an AWS CloudFormation template (`aws-vpc.yaml`) to provision a basic infrastructure setup in AWS, including a custom VPC, subnet, internet gateway, route table, security group, and a public EC2 instance.

## Features

- **Custom VPC** with DNS support and hostnames enabled
- **Public Subnet** in a specified Availability Zone
- **Internet Gateway** attached to the VPC
- **Route Table** with default route to the internet
- **Security Group** allowing SSH (port 22) from anywhere
- **EC2 Instance** (Amazon Linux 2) launched in the public subnet

## Prerequisites

- AWS account with permissions to create VPC, EC2, and related resources
- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) configured
- An existing EC2 Key Pair in your AWS region

## Usage

1. **Clone the repository:**
   ```sh
   git clone https://github.com/your-repo/cloudformation-vpc-ec2.git
   cd cloudformation-vpc-ec2
   ```

2. **Deploy the stack:**
   ```sh
   aws cloudformation create-stack \
     --stack-name my-vpc-ec2-stack \
     --template-body file://aws-vpc.yaml \
     --parameters ParameterKey=KeyName,ParameterValue=<YourKeyPairName> \
     --capabilities CAPABILITY_NAMED_IAM
   ```

3. **Find the EC2 Public IP:**
   - After deployment, the public IP is available in the CloudFormation Outputs section or via:
     ```sh
     aws cloudformation describe-stacks --stack-name my-vpc-ec2-stack \
       --query "Stacks[0].Outputs[?OutputKey=='InstancePublicIP'].OutputValue" --output text
     ```

## Template Parameters

- `KeyName`: Name of your existing EC2 Key Pair (required for SSH access)

## Security Notice

- The security group allows SSH access from any IP (`0.0.0.0/0`). For production, restrict this to trusted IP ranges.

## Cleanup

To delete all resources created by the stack:
```sh
aws cloudformation delete-stack --stack-name my-vpc-ec2-
