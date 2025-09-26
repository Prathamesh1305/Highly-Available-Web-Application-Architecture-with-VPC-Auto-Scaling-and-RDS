
Step 1: Create VPC and Network Infrastructure
1.1 Create VPC

Console Steps:

Go to VPC Dashboard → Create VPC Name: HA-vpc IPv4 CIDR: 10.0.0.0/16 IPv6 CIDR: No IPv6 Tenancy: Default
<img width="1900" height="898" alt="image" src="https://github.com/user-attachments/assets/baeaa922-c6df-4015-8147-b321d7534c2e" />

1.2 Create Subnets (4 subnets total)

Public Subnets:

Public Subnet AZ-A: 10.0.1.0/24 (us-east-1a) Public Subnet AZ-B: 10.0.2.0/24 (us-east-1b)

Private Subnets:

Private Subnet AZ-A: 10.0.11.0/24 (us-east-1a) Private Subnet AZ-B: 10.0.12.0/24 (us-east-1b)
<img width="1892" height="903" alt="image" src="https://github.com/user-attachments/assets/899a3667-f49a-4266-8d65-bcc9ab29237d" />

1.3 Create Internet Gateway

Create Internet Gateway → Name: HA-igw Attach to HA-vpc
<img width="1893" height="900" alt="image" src="https://github.com/user-attachments/assets/669fea01-1849-4093-b65a-c1b467317a46" />


1.4 Create NAT Gateways

Create NAT Gateway in each public subnet NAT-AZ-A in Public Subnet AZ-A NAT-AZ-B in Public Subnet AZ-B Allocate Elastic IPs for each
<img width="1894" height="897" alt="image" src="https://github.com/user-attachments/assets/c5e6e8f4-3086-4c5a-b919-b5d2a3519fdf" />

Step 2: Create Security Groups
2.1 ALB Security Group

Name: HA-alb-sg

Inbound Rules:

HTTP (80) - 0.0.0.0/0

HTTPS (443) - 0.0.0.0/0

2.2 EC2 Security Group

Name: HA-ec2-sg

Inbound Rules: HTTP (80) - ALB Security Group

SSH (22) - Your IP (for management)

2.3 Database Security Group

Name: HA-db-sg Inbound Rules:

MySQL (3306) - EC2 Security Group

2.4 EFS Security Group

Name: HA-efs-sg

Inbound Rules:

NFS (2049) - EC2 Security Group
<img width="1890" height="903" alt="image" src="https://github.com/user-attachments/assets/7d37949f-aada-4696-8cdb-10703badc193" />


Step 3: MySql Databas:-
=
3.1 Create DB Subnet Group

Go to RDS → Subnet Groups → Create

Name: HA-db-subnet-group

VPC: Select your VPC

Add both private subnets

3.2 Create MySql

RDS → Create Database

Engine: MySql

Version: Latest

Templates: Production

Cluster identifier:HA-MySql

Master username: admin

Master password: [secure-password]

Instance class: db.t3.medium (start small, can scale)

Multi-AZ: Yes

VPC: Your VPC

Subnet group: HA-db-subnet-group

Security group: HA-db-sg
<img width="1886" height="902" alt="image" src="https://github.com/user-attachments/assets/08705fb3-d92e-4b2b-8c5d-812230492643" />

<img width="1894" height="900" alt="image" src="https://github.com/user-attachments/assets/9c8adf9a-850f-41e5-bba3-ec92671e3e05" />


Step 4: Create EFS File System
=
4.1 Create EFS

EFS → Create File System

Name: HA-efs

VPC: Your VPC

Availability and Durability: Regional

Performance mode: General Purpose

Throughput mode: Provisioned

4.2 Create Mount Targets

Create mount targets in both private subnets

Security group: HA-efs-sg
<img width="1893" height="896" alt="image" src="https://github.com/user-attachments/assets/8094b315-b116-4612-9a91-0ffe5f1791fb" />

Step 5: Create Launch Template and Auto Scaling
=
5.1 Create Launch Template

bash# Name: HA-launch-template

AMI: Amazon Linux 2023

Instance type: t3.medium

Key pair: Your key pair

Security group: HA-ec2-sg

User Data Script:

bash#!/bin/bash

yum install httpd -y

systemctl restart httpd --now

sudo yum install -y httpd php php-mysqli mariadb

sudo yum install -y php php-cli php-mysqlnd php-pdo php-common php-gd php-xml

create index.html,db.php (Database Connection),register.php (User Registration),login.php (User Login)
<img width="1887" height="897" alt="image" src="https://github.com/user-attachments/assets/34e19a53-5bab-4b5f-9ea6-4651a767902a" />


5.2 Create Auto Scaling Group
=

EC2 → Auto Scaling Groups → Create

Name: HA-asg

Launch template: HA-launch-template

VPC: Your VPC

Subnets: Both private subnets

Desired: 2, Min: 2, Max: 6

Health check: ELB

Health check grace period: 300 seconds
<img width="1894" height="899" alt="image" src="https://github.com/user-attachments/assets/0c48ca36-8312-4d87-a209-367c43d3c840" />


Step 6: Create Application Load Balancer
=
6.1 Create ALB

EC2 → Load Balancers → Create ALB

Name: moodle-alb

Scheme: Internet-facing

VPC: Your VPC

Subnets: Both public subnets

Security group: HA-alb-sg
<img width="1890" height="902" alt="image" src="https://github.com/user-attachments/assets/8ab8193a-ec91-46cf-960b-a6901bb42a09" />

6.2 Create Target Group

Name: HA-targets

Protocol: HTTP

Port: 80

VPC: Your VPC

Health check path: /moodle/
<img width="1894" height="895" alt="image" src="https://github.com/user-attachments/assets/5394fc9e-2f89-4320-a8f7-8cc7a62a7e9a" />

Step 7 output**
7.1 Register Account
<img width="1919" height="908" alt="image" src="https://github.com/user-attachments/assets/909e1f26-87cb-4dfb-8800-1fd0459ca1d7" />





























