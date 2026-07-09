\# Day 02 – Creating an EC2 Security Group Using AWS CLI



\## Overview



Today's lab focused on creating and configuring an Amazon EC2 Security Group using the AWS Command Line Interface (AWS CLI).



A Security Group acts as a virtual firewall that controls inbound and outbound traffic for AWS resources such as EC2 instances. Instead of configuring firewall rules directly on a server, AWS allows these rules to be managed at the infrastructure level through Security Groups.



In this challenge, the objective was to create a new Security Group inside the default VPC and configure inbound rules to allow web traffic (HTTP) and remote administration (SSH).



\---



\## Lab Requirements



The security group needed to satisfy the following requirements.



| Requirement | Value |

|------------|-------|

| Security Group Name | datacenter-sg |

| Description | Security group for Nautilus App Servers |

| Region | us-east-1 |

| VPC | Default VPC |



Inbound rules:



| Protocol | Port | Source |

|----------|------|---------|

| HTTP | 80 | 0.0.0.0/0 |

| SSH | 22 | 0.0.0.0/0 |



\---



\# Understanding the Task



Before starting, I learned that every Security Group belongs to a VPC.



Since the challenge specifically asked to create the Security Group inside the \*\*default VPC\*\*, the first step was identifying the default VPC ID.



Without the VPC ID, AWS cannot create the Security Group.



\---



\# Step 1 – Configure AWS



The lab environment provided temporary AWS credentials.



To load them:



```bash

showcreds

```



Next, I configured the required AWS region.



```bash

aws configure set region us-east-1

```



This ensures all resources are created in the correct AWS region.



\---



\# Step 2 – Find the Default VPC



To locate the default VPC:



```bash

aws ec2 describe-vpcs \\

\--filters Name=isDefault,Values=true \\

\--query "Vpcs\[0].VpcId" \\

\--output text

```



Output:



```text

vpc-0757a4910a0b5489d

```



This VPC ID was required when creating the Security Group.



\---



\# Step 3 – Create the Security Group



Using the VPC ID, I created the Security Group.



```bash

aws ec2 create-security-group \\

\--group-name datacenter-sg \\

\--description "Security group for Nautilus App Servers" \\

\--vpc-id vpc-0757a4910a0b5489d

```



AWS returned a Security Group ID.



```text

sg-07f171aaef105a4b9

```



This confirmed that the Security Group had been created successfully.



\---



\# Step 4 – Configure HTTP Access



The first inbound rule allows HTTP traffic.



HTTP uses port \*\*80\*\*, which is required for web applications.



Command:



```bash

aws ec2 authorize-security-group-ingress \\

\--group-id sg-07f171aaef105a4b9 \\

\--protocol tcp \\

\--port 80 \\

\--cidr 0.0.0.0/0

```



Result:



```

Return: true

```



\---



\# Step 5 – Configure SSH Access



The second rule allows administrators to connect securely using SSH.



SSH uses port \*\*22\*\*.



Command:



```bash

aws ec2 authorize-security-group-ingress \\

\--group-id sg-07f171aaef105a4b9 \\

\--protocol tcp \\

\--port 22 \\

\--cidr 0.0.0.0/0

```



Result:



```

Return: true

```



\---



\# Step 6 – Verify the Security Group



Finally, I verified that the Security Group was created correctly.



```bash

aws ec2 describe-security-groups \\

\--group-ids sg-07f171aaef105a4b9

```



The output confirmed:



\- Security Group Name: \*\*datacenter-sg\*\*

\- Description: \*\*Security group for Nautilus App Servers\*\*

\- HTTP rule (TCP Port 80)

\- SSH rule (TCP Port 22)

\- Source CIDR: \*\*0.0.0.0/0\*\*



This confirmed that the Security Group matched all the lab requirements.



\---



\# Challenges Faced



This task did not produce any errors, but it introduced an important concept.



Initially, I assumed that a Security Group could be created directly. During the lab, I learned that AWS requires every Security Group to belong to a VPC. Therefore, identifying the default VPC was an essential first step before creating the Security Group.



I also learned that creating the Security Group is only part of the process. The required inbound rules must be added separately to allow network traffic.



\---



\# What I Learned



After completing this lab, I now understand:



\- What an AWS Security Group is

\- Why Security Groups are important for cloud security

\- The relationship between Security Groups and VPCs

\- How to create Security Groups using AWS CLI

\- How to configure inbound rules

\- How to verify AWS resources using CLI commands



\---



\# Conclusion



This lab strengthened my understanding of AWS network security and provided hands-on experience with the AWS CLI. I learned how Security Groups act as virtual firewalls and how properly configured inbound rules control access to cloud resources. This exercise also reinforced the importance of verifying infrastructure after deployment and following AWS security best practices.

