# AWS-bastion-private-ec2-access
##Secure SSH access to private EC2 Instance Connect

##Project Overview
This project demonstrates how to securely access ec2 instances in a private subnet that have no public access IP address using abastion host  deployed in apublic subnet and AWS EC2 instance connect.

#Architecture
-VPC with public and private subnets
-EC2 linux instances in the private subnet without a public IP address
-Bastion host instance in the public subnet with a public IP address
-security groups restricting SSH access

##Routing
-public subnet routes traffic to an internet gateway
-private subnet has no direct internet access


##Security Configuration

##Bastion Secuirty Group
