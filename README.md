# Secure Multi-AZ Access to Private EC2 Using a Bastion Host (AWS)

## Introduction
This project demonstrates how to securely access private EC2 instances deployed across multiple Availability Zones in AWS using a bastion host.

The private EC2 instances do not have public IP addresses and cannot be accessed directly from the internet. A bastion host placed in a public subnet acts as the only secure entry point.

---

## Project Objectives
- Design a secure AWS VPC architecture
- Separate public and private resources using subnets
- Deploy EC2 instances across multiple Availability Zones
- Access private EC2 instances using a bastion host
- Use EC2 Instance Connect for secure SSH access
- Apply AWS security best practices

---

## Architecture Overview
The architecture consists of:
- One Virtual Private Cloud (VPC)
- Four subnets (two public, two private)
- Two Availability Zones (`us-east-1a` and `us-east-1b`)
- One bastion host
- Four private EC2 instances
- Security groups controlling SSH access

### Availability Zone Distribution
- **us-east-1a**
  - Public Subnet 1 (Bastion Host)
  - Private Subnet 1 (2 EC2 instances)

- **us-east-1b**
  - Public Subnet 2
  - Private Subnet 2 (2 EC2 instances)

This design improves availability and fault tolerance.

---

## Network Configuration

### VPC
- **VPC CIDR Block:** `192.168.0.0/18`

### Subnets
| Subnet Name | Type | Availability Zone | CIDR Block |
|------------|------|------------------|-----------|
| Public Subnet 1 | Public | us-east-1a | `192.168.1.0/24` |
| Private Subnet 1 | Private | us-east-1a | `192.168.0.0/24` |
| Private Subnet 2 | Private | us-east-1b | `192.168.2.0/24` |
| Public Subnet 2 | Public | us-east-1b | `192.168.3.0/24` |

---

### Bastion Host
- Deployed in **Public Subnet 1 (us-east-1a)**
- Has a public IPv4 address
- Accessed using **EC2 Instance Connect**
- Acts as the single SSH entry point

### Private EC2 Instances
- 2 instances in **Private Subnet 1 (us-east-1a)**
- 2 instances in **Private Subnet 2 (us-east-1b)**
- No public IP addresses
- Accessible only from the bastion host

---

## Security Configuration

### Bastion Host Security Group
- **Inbound Rules**
- SSH (port 22) allowed from anywhere
- **Outbound Rules**
- All traffic allowed

### Private EC2 Security Group
  **Inbound Rules**
- SSH (port 22) allowed only from the bastion host security group
  **Outbound Rules**
- All traffic allowed

This ensures private EC2 instances are never exposed to the internet.

## Infrastructure Setup Method
All resources in this project were created **manually using the AWS Management Console**.


## Access Method Used

### Step 1: Connect to Bastion Host
The bastion host was accessed using **EC2 Instance Connect** from the AWS Management Console.  
This method injects a temporary SSH key into the bastion host.

---

### Step 2: SSH to Private EC2 Instances
From the bastion host terminal, private EC2 instances were accessed using their private IP addresses:

1. Copy the key pair associated with the private EC2 instances to the Bastion Host.
2. Secure the key pair by updating its file permissions:
   ```bash
   chmod 0400 <keypair>.pem
3. Then ssh to instances using their private IP addresses

  ```bash
ssh -i <keypair> ec2-user@<private-ip>

