# 🖥️ Deploying a WordPress Website on AWS EC2

This guide provides step-by-step instructions to deploy a WordPress website using an EC2 instance on AWS. It covers setting up the VPC, subnet, security group, and launching the EC2 instance with WordPress.

## 📋 Table of Contents

- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Step 1: Login to AWS](#step-1-login-to-aws)
- [Step 2: Set Up a VPC](#step-2-set-up-a-vpc)
- [Step 3: Add an Internet Gateway](#step-3-add-an-internet-gateway)
- [Step 4: Create a Subnet](#step-4-create-a-subnet)
- [Step 5: Set Up Route Tables](#step-5-set-up-route-tables)
- [Step 6: Configure a Security Group](#step-6-configure-a-security-group)
- [Step 7: Launch the EC2 Instance](#step-7-launch-the-ec2-instance)
- [Step 8: Access WordPress & Retrieve Credentials](#step-8-access-wordpress--retrieve-credentials)
- [Step 9: Log In to WordPress](#step-9-log-in-to-wordpress)
- [Clean Up](#clean-up)
- [Conclusion](#conclusion)

---

## Introduction

This guide will walk you through deploying a WordPress site on AWS using an EC2 instance. You’ll create a VPC, subnet, security group, and set up WordPress on your EC2 instance.

## Prerequisites

Before you begin, ensure you have:
- An AWS account
- Basic knowledge of AWS services and networking concepts

---

## Step 1: Login to AWS

1. **Access AWS Console**: Go to the AWS Management Console.
2. **Sign In**: Use your IAM credentials to log in.

---

## Step 2: Set Up a VPC

1. **Navigate to VPC**: In the AWS Console, search for **VPC** and select it.
2. **Create a VPC**:
   - **Name**: WordPress-VPC
   - **IPv4 CIDR**: 10.0.0.0/16
   - **Tenancy**: Default
3. **Save**: Click **Create VPC**.

4. **Edit DNS Settings**:
   - Go to **Actions** > **Edit VPC Settings**.
   - Enable DNS resolution and DNS hostnames.
   - Save changes.

---

## Step 3: Add an Internet Gateway

1. **Create an Internet Gateway**:
   - **Name**: WordPress-Ig
   - Click **Create**.
2. **Attach to VPC**:
   - Select **WordPress-VPC** and click **Attach**.

---

## Step 4: Create a Subnet

1. **Navigate to Subnets**: In the VPC dashboard, click **Subnets**.
2. **Create a Subnet**:
   - **Name**: WordPress-subnet-01
   - **VPC**: WordPress-VPC
   - **Availability Zone**: Select one from your region.
   - **IPv4 CIDR Block**: 10.0.1.0/24
3. **Save**: Click **Create Subnet**.

---

## Step 5: Set Up Route Tables

1. **Create Route Table**:
   - **Name**: WordPress-public-rt
   - **VPC**: WordPress-VPC
2. **Associate Route Table**:
   - Go to **Subnet Associations** > **Edit Subnet Associations**.
   - Select **WordPress-subnet-01** and save.
3. **Add Internet Route**:
   - Go to **Routes** > **Edit Routes** > **Add Route**.
   - **Destination**: 0.0.0.0/0
   - **Target**: WordPress-Ig
   - Save changes.

---

## Step 6: Configure a Security Group

1. **Create Security Group**:
   - **Name**: WordPress-SG
   - **Description**: Security group for WordPress EC2 instance.
   - **VPC**: WordPress-VPC
2. **Add Inbound Rules**:
   - **HTTP**: Port 80, Source 0.0.0.0/0
   - **HTTPS**: Port 443, Source 0.0.0.0/0
   - **SSH**: Port 22, Source 0.0.0.0/0
3. **Save**: Click **Create Security Group**.

---

## Step 7: Launch the EC2 Instance

1. **Go to EC2 Dashboard**: Search for **EC2** in the AWS Console.
2. **Launch Instance**:
   - **Name**: WordPress-EC2-Instance-01
   - **AMI**: Search for “WordPress Certified by Bitnami and Automattic” (Free Tier Eligible).
   - **Instance Type**: t2.micro
   - **Key Pair**: Create a new key pair.
   - **Network Settings**:
     - **VPC**: WordPress-VPC
     - **Subnet**: WordPress-subnet-01
     - **Auto-assign Public IP**: Enable
   - **Security Group**: Select WordPress-SG
3. **Launch**: Click **Launch Instance**.

---

## Step 8: Access WordPress & Retrieve Credentials

1. **Find Public IP**:
   - In the EC2 dashboard, locate your instance and copy its Public IPv4 address.
2. **Access WordPress**:
   - Open a browser and enter the IP address followed by `/wp-admin`.

---

## Step 9: Log In to WordPress

1. **Retrieve Credentials**:
   - Go to EC2 > **Actions** > **Monitor and Troubleshoot** > **Get System Log**.
   - Locate your WordPress credentials in the system log.
2. **Log In**:
   - Enter the default username `user` and the retrieved password on the WordPress login page.

---

## Clean Up

To avoid incurring charges:
1. **Terminate EC2 Instance**: Go to the EC2 dashboard and terminate the WordPress instance.
2. **Delete VPC**: Ensure all associated resources (subnets, internet gateway, route tables, security groups) are removed before deleting the VPC.

---

## Conclusion

By following this guide, you've successfully deployed a WordPress site on an EC2 instance within a custom VPC on AWS. This setup is foundational for building scalable, secure web applications in the cloud.

---

Let me know if you need any more adjustments!
