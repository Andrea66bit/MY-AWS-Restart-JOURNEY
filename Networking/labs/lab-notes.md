Networking Lab: AWS Virtual Private Cloud (VPC) Deployment and Security

Overview

This laboratory exercise details the creation and configuration of an Amazon Virtual Private Cloud (VPC), a logically isolated segment of the AWS cloud designed for secure resource deployment and granular network traffic control. Upon completion, the participant will have established a multi-tier custom VPC featuring public and private subnets, appropriate route tables, Internet and NAT gateways, and connectivity-tested EC2 instances.

Objective

Master the core components of AWS networking by implementing the following requirements:

Designing and deploying a VPC with a non-default Classless Inter-Domain Routing (CIDR) block.

Configuring highly available public and private subnets across multiple Availability Zones.

Establishing route tables and gateways to manage both internet ingress (to the public subnet) and secure egress (from the private subnet).

Launching Amazon Elastic Compute Cloud (EC2) instances in both tiers to validate the defined routing paths.

Applying layered security measures using stateful Security Groups and stateless Network Access Control Lists (NACLs).

Testing end-to-end network connectivity and isolation between public and private resources.

Prerequisites

An active AWS account with necessary permissions for VPC and EC2 service access.

Proficiency in fundamental networking concepts: subnetting, CIDR notation, routing protocols, and firewall types.

A pre-existing SSH key pair is required for secure administrative access to EC2 instances.

Procedural Steps

The following steps outline the required practical configuration process:

VPC Creation – Initiate the process via the VPC Dashboard and provision a new VPC utilizing a custom CIDR block (e.g., \texttt{10.0.0.0/16}).

Subnet Provisioning – Create dedicated public and private subnets (e.g., \texttt{10.0.1.0/24} and \texttt{10.0.2.0/24}) ensuring distribution across distinct Availability Zones.

Gateway Attachment – Attach an Internet Gateway (IGW) to the provisioned VPC.

Public Routing Configuration – Create a Public Route Table, explicitly associate it with the public subnet(s), and define a default route (\texttt{0.0.0.0/0}) directing traffic to the Internet Gateway (IGW).

NAT Gateway Deployment – Deploy a Network Address Translation (NAT) Gateway (which necessitates an Elastic IP) within the Public Subnet. This enables outbound internet connectivity for private instances.

Private Routing Configuration – Create a Private Route Table, associate it with the private subnet(s), and define a default route (\texttt{0.0.0.0/0}) pointing traffic toward the NAT Gateway.

Instance Deployment – Provision a Bastion Host EC2 instance in the public subnet and an Application Server EC2 instance in the private subnet.

Security Definition – Configure Security Groups (stateful firewall) to govern instance-level access (e.g., permit SSH from the Bastion Host to the Application Server). Establish Network ACLs (stateless firewall) to enforce subnet-level traffic policies.

Connectivity Validation – Securely connect to the public Bastion Host via SSH, and subsequently, utilize the Bastion Host to SSH into the private Application Server (using its private IP). Confirm that the private instance can successfully access external network resources (e.g., \texttt{ping} or \texttt{curl}).

Clean Up

To mitigate accrued AWS resource charges, ensure the following components are systematically terminated or deleted in the specified order:

EC2 Instances (and any associated Elastic IP resources).

NAT Gateway (requires a successful deletion status before proceeding).

Internet Gateway (must be successfully detached prior to deletion).

Subnets and associated Route Tables.

The VPC resource itself.

Summary of Learning Outcomes

This laboratory exercise provided practical experience in designing and deploying a secure, multi-tiered VPC infrastructure. It reinforced critical networking competencies, including CIDR subnetting, strategic routing design, and the implementation of layered security controls (Security Groups versus Network ACLs) essential for modern cloud architecture management.
