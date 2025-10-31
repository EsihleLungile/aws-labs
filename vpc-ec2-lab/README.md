# AWS VPC + EC2 Lab

## 🎯 Objective
Build a secure VPC with public & private subnets and deploy EC2 instances.

## 🏗 Architecture
- 1 VPC
- 2 Public Subnets
- 2 Private Subnets
- Internet Gateway + NAT Gateway
- 2 EC2 Instances (Linux)
- IAM Roles for EC2

## 🛠 Commands

### Create VPC
aws ec2 create-vpc --cidr-block 10.0.0.0/16

### Launch EC2
aws ec2 run-instances \
 --image-id ami-xxxx \
 --count 1 \
 --instance-type t2.micro \
 --key-name mykey \
 --security-group-ids sg-xxxxx \
 --subnet-id subnet-xxxxx
