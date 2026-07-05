Day 01 - AWS Key Pair Creation (DevOps Lab Task 1)



Task Overview

The goal of this task was to create an AWS EC2 key pair as part of the Nautilus DevOps migration lab.



Requirements

\- Key Pair Name: devops-kp

\- Key Pair Type: rsa

\- Region: us-east-1

\- Must be created using AWS CLI



Tools Used

\- AWS CLI v2

\- AWS Cloud Lab Environment

\- EC2 Service (Key Pairs)



Steps Followed



1\. Logged into AWS CLI environment using provided credentials:

showcreds



This automatically configured AWS CLI access.



2\. Set the correct AWS region:

aws configure set region us-east-1



Mistake Made

Initially, I tried to create the key pair using a multi-line command:



aws ec2 create-key-pair \\

\--key-name devops-kp \\

\--key-type rsa \\

\--query 'KeyMaterial' \\

\--output text > devops-kp.pem



Error received:

aws: error: the following arguments are required: --key-name



Root Cause

\- Multi-line command was not parsed correctly in the terminal

\- Backslashes caused arguments to break

\- AWS CLI did not receive full input correctly



Fix Applied

I corrected the command into a single-line format:



aws ec2 create-key-pair --key-name devops-kp --key-type rsa --query KeyMaterial --output text > devops-kp.pem



File Verification

ls



Output:

devops-kp.pem



Set Permissions

chmod 400 devops-kp.pem



Verification in AWS

aws ec2 describe-key-pairs --key-names devops-kp



Result:

KeyPair was successfully created and is available in AWS EC2.



KeyPair Details:

\- Key Name: devops-kp

\- Key Type: rsa

\- Region: us-east-1



Final Result

\- Key pair successfully created

\- Private key downloaded as devops-kp.pem

\- Verified in AWS console and CLI



Key Learnings

\- AWS CLI commands must be properly formatted

\- Single-line commands are safer in lab environments

\- Always verify resources using describe commands

\- Private key files must be secured using chmod 400



Conclusion

This task helped understand AWS EC2 key pair creation and basic AWS CLI troubleshooting in a DevOps environment.

