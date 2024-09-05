# Amazon VPC Traffic Flow and Security Configuration

## Project Overview
This project demonstrates how to set up a secure network using Amazon Virtual Private Cloud (VPC) with subnets, routing, and access controls. The project focuses on configuring security groups, network ACLs, route tables, and Internet gateways to manage traffic and secure cloud resources in AWS.

## Table of Contents
1. [What is Amazon VPC?](#what-is-amazon-vpc)
2. [Project Setup Steps](#project-setup-steps)
3. [Security Groups and Network ACLs](#security-groups-and-network-acls)
4. [Route Tables and Internet Gateway](#route-tables-and-internet-gateway)
5. [Key Learnings](#key-learnings)

## What is Amazon VPC?
Amazon VPC (Virtual Private Cloud) is a service that allows you to create isolated and customizable networks in AWS. It enables you to control IP addresses, subnets, and security settings, making it easier to manage cloud resources securely.

In this project, I created an Amazon VPC to:
- Configure subnets.
- Manage routing tables.
- Control access with security groups and network ACLs.

## Project Setup Steps

### 1. Create a VPC
- **VPC Name**: `my-secure-vpc`
- **IPv4 CIDR block**: `10.0.0.0/16`

    ![S1](https://github.com/user-attachments/assets/a3477718-8aa1-4760-9861-4b69199b8384)

This creates an isolated virtual network in AWS with a defined IP address range.

### 2. Configure Subnets
- **Public Subnet**: `10.0.0.0/24`

    ![S2](https://github.com/user-attachments/assets/b0e77252-d363-4aaa-bef9-4f91dd6c320c)

  
Subnets divide the VPC into smaller IP ranges. The public subnet is configured for resources that need internet access, while the private subnet is for internal resources.

### 3. Set up an Internet Gateway
- **Internet Gateway Name**: `Project IG`

  ![S3](https://github.com/user-attachments/assets/dcd0b763-112e-4306-ac2b-5e1679fab448)

  
An Internet Gateway allows resources in the VPC to communicate with the internet.

### 4. Attach the Internet Gateway to the VPC
- Attached `Project IG` to `Project VPC` to allow internet-bound traffic from the VPC.

### 5. Configure Route Tables
- **Public Route Table**: 
  - Route: `0.0.0.0/0` → Target: `Internet Gateway (Project IG)`
  - Subnet Association: `Public Subnet (10.0.0.0/24)`

    ![S1](https://github.com/user-attachments/assets/6cccc112-b9d2-4987-8255-ca9df87f2e4d)
  
This route allows traffic from the public subnet to access the internet through the Internet Gateway.

### 6. Configure Security Groups
- **Inbound Rule**: Allow HTTP traffic (Port 80) from `0.0.0.0/0`
  - This allows any IP address to access resources using HTTP.
- **Outbound Rule**: Allow TCP protocol outbound traffic

  ![S2](https://github.com/user-attachments/assets/c4c84182-a04b-4527-aaa8-066696f471b0)

  
Security Groups act as virtual firewalls for controlling traffic to and from EC2 instances within the VPC.

### 7. Configure Network ACLs
- **Custom ACL**: 
  - **Inbound Rule**: Allow HTTP traffic from `0.0.0.0/0`
  - **Outbound Rule**: Allow all outbound traffic
  - **Default Action**: Deny all other traffic

    ![S3](https://github.com/user-attachments/assets/0d1b1b42-00e3-4f73-bf46-96e9f2e20630)

Network ACLs are stateless firewalls applied at the subnet level. In contrast to security groups, they apply to all resources within a subnet and control traffic in and out.

## Security Groups and Network ACLs

### Security Groups
Security Groups are stateful and manage traffic for individual instances. I set up the following rules for the security group:
- **Inbound**: HTTP traffic (port 80) is allowed from any IP address (`0.0.0.0/0`).
- **Outbound**: All traffic is allowed by default.

### Network ACLs
Network ACLs are stateless, applying rules to an entire subnet. The custom ACL in this project denied all traffic by default, requiring specific rules to allow HTTP traffic.

- **Inbound Rule**: Allow HTTP (port 80) traffic from `0.0.0.0/0`.
- **Outbound Rule**: Allow all outbound traffic.

## Route Tables and Internet Gateway

### Route Tables
Route tables manage how traffic moves within the VPC and to external networks. In this project:
- The **public route table** had a route that allowed outbound internet traffic from the public subnet via the Internet Gateway.

### Internet Gateway
An Internet Gateway is attached to the VPC to allow traffic from the public subnet to flow to the internet. The route for internet-bound traffic is defined as:
- **Destination**: `0.0.0.0/0`
- **Target**: `Internet Gateway (my-igw)`

## Key Learnings
- **Security Groups vs. Network ACLs**: Security Groups are stateful and easier to manage, while Network ACLs are stateless and more granular.
- **Route Tables**: Creating the correct route to the Internet Gateway is crucial for allowing internet-bound traffic from a public subnet.
- **Network ACL Complexity**: Configuring Network ACLs and ensuring they don't conflict with Security Group rules was more challenging than expected.
