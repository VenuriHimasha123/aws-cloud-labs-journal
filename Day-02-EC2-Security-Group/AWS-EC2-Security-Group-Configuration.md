# Day 02 - Creating and Configuring an AWS EC2 Security Group

## Task Overview

As part of my AWS Cloud and DevOps learning journey, this task focused on creating and configuring an Amazon EC2 Security Group using AWS CLI.

The Nautilus DevOps team is gradually migrating their infrastructure to AWS. Instead of performing a large-scale migration at once, the infrastructure is divided into smaller tasks. This approach helps reduce risk, improve control, and allow each component to be configured and verified separately.

In this task, the objective was to prepare network security configuration for future application servers by creating a Security Group with required inbound access rules.

---

# Objective

The main objectives of this task were:

- Create a Security Group inside the default VPC
- Configure the Security Group with a meaningful name and description
- Allow HTTP traffic for web applications
- Allow SSH traffic for remote server administration
- Verify the Security Group configuration using AWS CLI

---

# AWS Concepts Learned

## What is a Security Group?

An AWS Security Group is a virtual firewall that controls network traffic for AWS resources such as EC2 instances.

It defines:

- Inbound rules: Control incoming traffic to resources
- Outbound rules: Control outgoing traffic from resources

Security Groups work at the instance level and help protect cloud infrastructure by allowing only required network access.

---

## Why are Security Groups Important?

In a production environment, servers should not be exposed to unnecessary traffic.

For example:

- A web server may need HTTP access on port 80
- An administrator may need SSH access on port 22
- Other unused ports should remain blocked

By configuring Security Groups properly, organizations can reduce security risks.

---

# Task Requirements

The Security Group needed to be created with the following specifications:

| Configuration | Value |
|---|---|
| Security Group Name | datacenter-sg |
| Description | Security group for Nautilus App Servers |
| Region | us-east-1 |
| VPC | Default VPC |

## Required Inbound Rules

| Type | Protocol | Port | Source |
|---|---|---|---|
| HTTP | TCP | 80 | 0.0.0.0/0 |
| SSH | TCP | 22 | 0.0.0.0/0 |

---

# Environment Setup

The AWS lab provided temporary credentials through the AWS client machine.

The credentials were loaded using:

```bash
showcreds
```

The required AWS region was configured:

```bash
aws configure set region us-east-1
```

This ensured that all AWS resources were created in the correct region.

---

# Implementation Steps

## Step 1 - Identify the Default VPC

Before creating a Security Group, it is necessary to know which VPC it belongs to.

I retrieved the default VPC ID using:

```bash
aws ec2 describe-vpcs \
--filters Name=isDefault,Values=true \
--query "Vpcs[0].VpcId" \
--output text
```

Output:

```
vpc-0757a4910a0b5489d
```

This VPC ID was used during Security Group creation.

---

# Step 2 - Create the Security Group

The Security Group was created using:

```bash
aws ec2 create-security-group \
--group-name datacenter-sg \
--description "Security group for Nautilus App Servers" \
--vpc-id vpc-0757a4910a0b5489d
```

AWS returned:

```
GroupId:
sg-07f171aaef105a4b9
```

This confirmed that the Security Group was successfully created.

---

# Step 3 - Add HTTP Inbound Rule

The first required rule was allowing HTTP traffic.

HTTP is commonly used for web applications and runs on port 80.

Command:

```bash
aws ec2 authorize-security-group-ingress \
--group-id sg-07f171aaef105a4b9 \
--protocol tcp \
--port 80 \
--cidr 0.0.0.0/0
```

Result:

```json
{
    "Return": true
}
```

The rule was successfully added.

---

# Step 4 - Add SSH Inbound Rule

The second rule allowed SSH access.

SSH is used for secure remote access to Linux servers and runs on port 22.

Command:

```bash
aws ec2 authorize-security-group-ingress \
--group-id sg-07f171aaef105a4b9 \
--protocol tcp \
--port 22 \
--cidr 0.0.0.0/0
```

Result:

```json
{
    "Return": true
}
```

The SSH rule was successfully added.

---

# Step 5 - Verify Security Group Configuration

After adding the rules, I verified the Security Group:

```bash
aws ec2 describe-security-groups \
--group-ids sg-07f171aaef105a4b9
```

Verification confirmed:

Security Group:

```
Name:
datacenter-sg

Description:
Security group for Nautilus App Servers
```

Inbound permissions:

```
HTTP
Protocol: TCP
Port: 80
Source: 0.0.0.0/0


SSH
Protocol: TCP
Port: 22
Source: 0.0.0.0/0
```

---

# Problems Faced and Troubleshooting

## Problem 1 - Accidentally Cancelling Commands

During the task execution, I used:

```
Ctrl + C
```

multiple times while entering commands.

Example:

```
^C
```

### Cause

Ctrl + C interrupts the currently running command.

### Solution

I re-entered the command carefully and verified the previous step before continuing.

---

## Problem 2 - Understanding the VPC Requirement

Initially, the relationship between Security Groups and VPCs was unclear.

### Issue

A Security Group cannot exist independently. It must belong to a VPC.

### Solution

I first identified the default VPC:

```bash
aws ec2 describe-vpcs --filters Name=isDefault,Values=true
```

Then used the VPC ID while creating the Security Group.

### Learning

AWS resources often have dependencies. Understanding those dependencies before creating resources prevents errors.

---

# Verification Summary

The final AWS configuration was:

```
Security Group:
datacenter-sg

Security Group ID:
sg-07f171aaef105a4b9

VPC:
vpc-0757a4910a0b5489d

Region:
us-east-1
```

Configured inbound rules:

```
HTTP  - TCP Port 80  - 0.0.0.0/0
SSH   - TCP Port 22  - 0.0.0.0/0
```

---

# Key Learnings

Through this task, I gained practical experience with:

- AWS EC2 Security Groups
- VPC and Security Group relationships
- AWS CLI resource creation
- Configuring inbound firewall rules
- Understanding network access control
- Troubleshooting AWS CLI workflows
- Verifying cloud infrastructure after deployment

---

# Final Outcome

The Security Group was successfully created and configured according to the requirements.

The completed setup provides:

- Web access through HTTP port 80
- Administrative access through SSH port 22
- Proper network configuration for future AWS application servers

This task improved my understanding of AWS networking fundamentals and strengthened my confidence in managing cloud infrastructure using AWS CLI.

