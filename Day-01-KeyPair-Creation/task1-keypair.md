Day 01 - AWS Key Pair Creation (DevOps Lab Task 1)

1. Introduction
This document describes the completion of Day 01 of the AWS DevOps learning journey. The task focuses on creating an EC2 key pair using AWS CLI as part of a structured cloud migration lab environment provided in the Nautilus DevOps setup.

The purpose of this task is to understand how AWS authentication using key pairs works and how to manage EC2 access securely using CLI-based operations.

---

2. Objective
The main objective of this task was to:
- Create an AWS EC2 key pair using AWS CLI
- Ensure the key pair is generated with RSA encryption type
- Store the private key securely in a .pem file
- Verify the key pair creation in AWS
- Perform all operations in the us-east-1 region

---

3. Prerequisites
Before starting the task, the following were provided:
- AWS Cloud Lab access credentials
- AWS CLI configured environment
- Access to AWS client terminal
- Permission to create EC2 resources

---

4. AWS Configuration Steps
The AWS credentials were loaded using the following command:

showcreds

This automatically configured AWS CLI authentication for the session.

Next, the default region was set to ensure resources were created in the correct location:

aws configure set region us-east-1

---

5. Task Execution

5.1 Initial Attempt (Incorrect Approach)
The first attempt to create the key pair used a multi-line command:

aws ec2 create-key-pair \
--key-name devops-kp \
--key-type rsa \
--query 'KeyMaterial' \
--output text > devops-kp.pem

This resulted in an error:

aws: error: the following arguments are required: --key-name

5.2 Root Cause of Error
After analysis, the issue was identified as:
- Improper handling of multi-line command in the AWS CLI environment
- Line continuation characters causing argument parsing failure
- AWS CLI not receiving complete command parameters

---

5.3 Corrected Implementation
The issue was resolved by converting the command into a single-line format:

aws ec2 create-key-pair --key-name devops-kp --key-type rsa --query KeyMaterial --output text > devops-kp.pem

This ensured that all parameters were passed correctly to AWS CLI.

---

6. File Handling and Security

After successful execution, the private key file was generated:

devops-kp.pem

File verification was done using:

ls

To ensure security of the private key, permissions were restricted:

chmod 400 devops-kp.pem

This step is important to prevent unauthorized access to the private key file.

---

7. Verification of Key Pair in AWS

To confirm successful creation of the key pair, the following command was executed:

aws ec2 describe-key-pairs --key-names devops-kp

The output confirmed:
- Key pair exists in AWS
- Key name is devops-kp
- Key type is RSA
- Creation timestamp is recorded in AWS EC2

---

8. Final Result
The task was successfully completed with the following outcomes:

- AWS EC2 key pair "devops-kp" created successfully
- RSA key type was used as required
- Private key downloaded and stored as devops-kp.pem
- Key pair verified using AWS CLI
- Proper security permissions applied to key file
- All operations performed in us-east-1 region

---

9. Key Learnings
From this task, the following concepts were learned:

- AWS EC2 key pair creation using AWS CLI
- Importance of correct command formatting in CLI environments
- Difference between successful execution and partial command failure
- Secure handling of private key files using file permissions
- Verification of AWS resources using describe commands

---

10. Conclusion
This task provided hands-on experience with AWS EC2 key pair management using CLI. It strengthened understanding of AWS authentication mechanisms and reinforced best practices for secure key handling and command-line troubleshooting in a DevOps environment.


Conclusion

This task helped understand AWS EC2 key pair creation and basic AWS CLI troubleshooting in a DevOps environment.

