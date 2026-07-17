# AWS Cloud Lab - Day 06
# Task: Create an EC2 Instance with Required Configuration

---

## Task Explanation

The objective of this task was to create an Amazon EC2 instance
with specific requirements.

The Nautilus DevOps team is migrating infrastructure to AWS
gradually. As part of this migration process, a new EC2 virtual
machine needed to be created.

An EC2 instance is a virtual server provided by AWS that can be
used to run applications, services, and workloads in the cloud.

---

## Required EC2 Configuration

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

# Steps Followed

## Step 1: Verify AWS Authentication

Command used:

```bash
aws sts get-caller-identity
