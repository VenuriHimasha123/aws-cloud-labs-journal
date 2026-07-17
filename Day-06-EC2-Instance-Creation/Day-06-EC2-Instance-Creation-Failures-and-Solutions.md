# AWS Cloud Labs - Day 06

# Create an EC2 Instance with Required Configuration

---

# 1. Task Description

The Nautilus DevOps team is preparing to migrate part of their
infrastructure to AWS Cloud.

Instead of moving the complete infrastructure at once, the team
is dividing the migration process into smaller tasks.

This approach is useful in DevOps because:

- Large changes become easier to manage
- Problems can be identified quickly
- Risk is reduced
- Each component can be tested separately
- Infrastructure changes become more controlled

For this task, the requirement was to create an Amazon EC2 instance
with specific configurations.

---

# 2. Understanding Amazon EC2

## What is EC2?

Amazon EC2 (Elastic Compute Cloud) is an AWS service that provides
virtual servers in the cloud.

A traditional server requires:

- Hardware purchase
- Data center maintenance
- Manual configuration

With EC2, AWS manages the physical infrastructure while users
create and manage virtual servers.

EC2 instances can be used for:

- Web application hosting
- Backend API deployment
- Database servers
- Testing environments
- Development environments

An EC2 instance contains:

- CPU
- Memory
- Storage
- Operating system
- Network configuration

---

# 3. Task Requirements

The EC2 instance should have:

| Requirement | Value |
|---|---|
| Instance Name | devops-ec2 |
| Operating System | Amazon Linux AMI |
| Instance Type | t2.micro |
| Key Pair | devops-kp |
| Key Pair Type | RSA |
| Security Group | Default Security Group |
| Region | us-east-1 |

---

# 4. AWS Region Explanation

## What is an AWS Region?

AWS has multiple geographical locations around the world.
These locations are called Regions.

Examples:

- us-east-1 → US East (N. Virginia)
- ap-south-1 → Asia Pacific (Mumbai)
- eu-west-1 → Europe

Each AWS Region contains multiple Availability Zones.

Resources created in one region are separate from resources
created in another region.

The task required:

Therefore all AWS commands were executed in us-east-1.

---

# Step 1: Verify AWS Credentials

## Why verify credentials?

Before creating cloud resources, we need to confirm that:

- AWS CLI is connected
- Correct AWS account is being used
- Correct IAM user is active



User:
kk_labs_user_142444
