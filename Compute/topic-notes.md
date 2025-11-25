Networking in the AWS "program" (referring to the suite of services) is about **virtual network components, topologies, and configurations** that run on Amazon Web Services' physical infrastructure. It allows you to define and manage your networks as software, connecting your cloud resources securely and efficiently.

The core service that forms the foundation of AWS networking is the **Amazon Virtual Private Cloud (VPC)**.

---

##  Core AWS Networking Concepts

Understanding AWS networking is crucial for building scalable, secure, and highly available cloud solutions.

### 1. Amazon Virtual Private Cloud (VPC)
The **VPC** is the foundation. It's a logically isolated section of the AWS Cloud where you can launch AWS resources (like virtual servers, called EC2 instances) in a virtual network that you define. You have complete control over:
* **IP Address Ranges:** You define your own private IPv4 and/or IPv6 address ranges using **CIDR blocks** (e.g., $10.0.0.0/16$).
* **Subnets:** You divide your VPC into one or more subnets, which are ranges of IP addresses within the VPC. A subnet must reside entirely within a single **Availability Zone (AZ)**.
    * **Public Subnet:** Resources here can access the public internet via an **Internet Gateway (IGW)**.
    * **Private Subnet:** Resources here are isolated from the public internet. They can access the internet using a **NAT Gateway** (to download updates, for example), but are not publicly reachable. 
* **Route Tables:** These contain a set of rules, called routes, that determine where network traffic from your subnet or gateway is directed.

### 2. Security Layers
AWS provides two main ways to secure your network traffic at different levels:
* **Security Groups (SGs):** These act as a virtual firewall for your **instance** (e.g., an EC2 server). They are **stateful** (if you allow outbound traffic, the corresponding inbound return traffic is automatically allowed). They operate on an **"allow-only"** basis.
* **Network Access Control Lists (NACLs):** These are an optional layer of security for your **subnets**. They are **stateless** (return traffic must be explicitly allowed) and support both **"allow"** and **"deny"** rules.

### 3. Connectivity Services
These services connect your VPCs to each other, to the public internet, or to your on-premises data centers:

| Service | Purpose |
| :--- | :--- |
| **Internet Gateway (IGW)** | Enables communication between resources in a public subnet and the public internet. |
| **VPC Peering** | Connects two VPCs to each other so they can communicate as if they were in the same network. (Not transitive) |
| **AWS Transit Gateway** | Acts as a central hub to connect multiple VPCs and on-premises networks, simplifying network architecture at scale. |
| **AWS Direct Connect** | Establishes a dedicated, private, physical network connection from your data center or office to AWS, bypassing the public internet for consistent, high-bandwidth performance. |
| **AWS Site-to-Site VPN** | Creates an encrypted connection between your on-premises network and your VPC over the public internet. |
| **AWS PrivateLink** | Allows private connectivity between VPCs and supported AWS services (or other customers' services) without exposing traffic to the public internet. |

### 4. Content Delivery and DNS
* **Amazon Route 53:** A highly available and scalable **Domain Name System (DNS)** web service that routes end users to your applications by translating domain names (like `example.com`) into IP addresses.
* **Amazon CloudFront:** AWS's **Content Delivery Network (CDN)** service that securely delivers data, videos, applications, and APIs globally with low latency by caching content at **Edge Locations** closer to your users.

***

You can learn the basics of networking using AWS in this video. [AWS Networking Basics For Programmers | Hands On](https://www.youtube.com/watch?v=2doSoMN2xvI)


http://googleusercontent.com/youtube_content/0
