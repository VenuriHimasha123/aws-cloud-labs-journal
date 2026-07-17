# AWS Cloud Labs - Day 06
# Create an EC2 Instance with Required Configuration

---

## 1. Task Description

The Nautilus DevOps team is preparing to migrate part of their infrastructure to AWS Cloud.

Instead of moving the complete infrastructure at once, the team is dividing the migration process into smaller and manageable tasks.

This approach is useful in DevOps because:
*   Large changes become easier to manage
*   Problems can be identified quickly
*   Risk is reduced
*   Each component can be tested separately
*   Infrastructure changes become more controlled

For this task, the requirement was to create an Amazon EC2 instance with specific configurations.

---

## 2. Understanding Amazon EC2

### What is EC2?

Amazon EC2 (Elastic Compute Cloud) is an AWS service that provides virtual servers in the cloud.

A traditional server requires:
*   Hardware purchasing
*   Data center management
*   Physical maintenance
*   Manual configuration

With Amazon EC2:
*   AWS manages the physical hardware
*   Users can create servers whenever needed
*   Resources can be scaled according to requirements

EC2 instances are commonly used for:
*   Hosting websites
*   Running backend applications
*   Deploying APIs
*   Database servers
*   Development environments
*   Testing environments

An EC2 instance contains:
*   CPU
*   Memory
*   Storage
*   Operating System
*   Network Configuration

---

## 3. Task Requirements

The EC2 instance should have the following configuration:

| Requirement | Value |
|---|---|
| **Instance Name** | devops-ec2 |
| **Operating System** | Amazon Linux AMI |
| **Instance Type** | t2.micro |
| **Key Pair Name** | devops-kp |
| **Key Pair Type** | RSA |
| **Security Group** | Default Security Group |
| **AWS Region** | us-east-1 |

---

## 4. AWS Region Explanation

### What is an AWS Region?

AWS has multiple geographical locations around the world. These locations are called Regions.

Examples:
*   **us-east-1** → US East (N. Virginia)
*   **ap-south-1** → Asia Pacific (Mumbai)
*   **eu-west-1** → Europe (Ireland)

Each region contains multiple Availability Zones. AWS resources created in one region are separate from resources created in another region.

The task required: **Region: us-east-1**
Therefore, all resources were created in the us-east-1 region.

---

## Step 1: Verify AWS Credentials

### Why verify credentials?
Before creating AWS resources, it is important to confirm:
*   AWS CLI is connected correctly
*   Correct AWS account is being used
*   Correct IAM user is active

If incorrect credentials are used, resources may be created in the wrong AWS account.

### Command
```bash
aws sts get-caller-identity
```

**Explanation:**
AWS STS (Security Token Service) provides information about the current AWS identity. It displays the AWS Account ID, IAM User, and ARN.

**Output:**
```text
Account: 374699107768
User: kk_labs_user_142444
```
*Authentication was successful.*

---

## Step 2: Verify AWS Region

### Command
```bash
aws configure get region
```

**Explanation:**
AWS CLI stores configuration information such as access credentials, default region, and output format. This command checks the currently configured AWS region.

**Output:**
```text
us-east-1
```
*The correct region was confirmed.*

---

## Step 3: Find Amazon Linux AMI

### What is AMI?
AMI means **Amazon Machine Image**. An AMI is a template used to create EC2 instances. It contains the operating system, software packages, and configuration settings (e.g., Amazon Linux, Ubuntu, Windows Server).

When launching an EC2 instance, AWS requires an AMI ID (Example: `ami-00948338a4aeec604`).

### Challenge 1: Finding the Correct AMI
**Problem:**
The task says: *"Use Amazon Linux AMI"*. However, AWS cannot create an EC2 instance using only the name "Amazon Linux". AWS requires the exact AMI ID. Also, AMI IDs are different for each AWS region.

**Solution:**
Used AWS CLI to search Amazon Linux AMIs.

### Command
```bash
aws ec2 describe-images \
--owners amazon \
--filters "Name=name,Values=al2023-ami-*" \
--query "Images[*].[ImageId,Name]" \
--output table
```

**Command Explanation:**
*   `describe-images`: Retrieves available AMIs.
*   `--owners amazon`: Searches only official Amazon images.
*   `--filters`: Filters results to Amazon Linux 2023 images.
*   `--query`: Displays only required information.
*   `--output table`: Displays results in readable table format.

**Selected AMI:**
AMI ID: `ami-00948338a4aeec604`

> **Learning:** AMI IDs are region-specific. Always select an AMI from the required AWS region.

---

## Step 4: Create RSA Key Pair

### What is an EC2 Key Pair?
A key pair is used for secure authentication when accessing an EC2 instance. It contains two keys:
1.  **Public Key:** Stored by AWS, attached to the EC2 instance.
2.  **Private Key:** Downloaded by the user, used for authentication.

The private key must be protected and should not be shared.

### Requirement
*   **Key Pair Name:** devops-kp
*   **Type:** RSA

### Challenge 2: Key Pair Requirement
**Problem:** EC2 instances require secure authentication. Without a key pair, users cannot securely connect to the server.
**Solution:** Created the RSA key pair `devops-kp.pem`.

### Command
```bash
aws ec2 create-key-pair \
--key-name devops-kp \
--key-type rsa \
--query "KeyMaterial" \
--output text > devops-kp.pem
```

**Command Explanation:**
*   `create-key-pair`: Creates a new EC2 key pair.
*   `--key-name`: Defines the key pair name.
*   `--key-type rsa`: Creates an RSA encryption key.
*   `--query "KeyMaterial"`: Retrieves the private key content.
*   `> devops-kp.pem`: Stores the private key locally.

---

## Step 5: Find Default VPC

### What is VPC?
VPC means **Virtual Private Cloud**. A VPC is a private network environment inside AWS. It controls IP address ranges, subnets, network communication, and security groups. Every EC2 instance runs inside a VPC.

### Command
```bash
aws ec2 describe-vpcs \
--filters Name=isDefault,Values=true \
--query "Vpcs[0].VpcId" \
--output text
```

**Result:**
```text
vpc-0cb55cc2674b9003a
```

---

## Step 6: Find Default Security Group

### What is a Security Group?
A Security Group is a virtual firewall for AWS resources. It controls:
*   **Inbound Traffic:** Traffic coming into the EC2 instance (e.g., SSH connection, HTTP requests).
*   **Outbound Traffic:** Traffic leaving the EC2 instance.

### Challenge 3: Selecting Correct Security Group
**Problem:** The task specifically required: *"Attach the default security group"*. Using another security group could cause validation failure.
**Solution:** Retrieved the default security group ID.

### Command
```bash
aws ec2 describe-security-groups \
--filters Name=vpc-id,Values=vpc-0cb55cc2674b9003a Name=group-name,Values=default \
--query "SecurityGroups[0].GroupId" \
--output text
```

**Result:**
```text
sg-0ea35d6949067a0e8
```

---

## Step 7: Launch EC2 Instance

### Command
```bash
aws ec2 run-instances \
--image-id ami-00948338a4aeec604 \
--instance-type t2.micro \
--key-name devops-kp \
--security-group-ids sg-0ea35d6949067a0e8 \
--tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=devops-ec2}]'
```

**Command Explanation:**
*   `--image-id`: Defines the OS image (Amazon Linux 2023).
*   `--instance-type`: Hardware capacity. `t2.micro` is suitable for learning environments, testing, and small applications.
*   `--key-name`: Attaches the created key pair (`devops-kp`).
*   `--security-group-ids`: Attaches the default security group (`sg-0ea35d6949067a0e8`).
*   `--tag-specifications`: Adds a name tag (`Name=devops-ec2`). AWS uses tags to organize and identify resources.

### Final Created EC2 Configuration
*   **Instance Name:** devops-ec2
*   **Operating System:** Amazon Linux 2023
*   **Instance Type:** t2.micro
*   **Key Pair:** devops-kp
*   **Security Group:** Default Security Group
*   **Region:** us-east-1

---

## Challenges and Solutions Summary

| Challenge | Problem | Solution |
|---|---|---|
| **Understanding task** | Migration description was large | Extracted actual AWS requirements |
| **AMI selection** | EC2 requires exact AMI ID | Used `describe-images` command |
| **Key pair creation** | Secure authentication required | Created RSA key pair |
| **Security group selection** | Required default group | Retrieved default security group |
| **Instance naming** | EC2 does not have a direct name field | Used AWS tags (`--tag-specifications`) |

---

## DevOps Concepts Learned

1. **Cloud Infrastructure Provisioning:** Learned how to create AWS resources using AWS CLI.
2. **Infrastructure Automation:** AWS CLI commands can later be automated using Shell scripts, Terraform, or CI/CD pipelines.
3. **Cloud Networking:** Learned about AWS Regions, VPC, and Security Groups.
4. **Cloud Security:** Learned about Key pairs, authentication, and security group control.
5. **Resource Management:** Learned how AWS tags are used to organize cloud resources.

---

## Task Status
**Completed Successfully** ✅
AWS Day 06 EC2 Instance Creation task completed.
```bash
aws sts get-caller-identity

The task required:

