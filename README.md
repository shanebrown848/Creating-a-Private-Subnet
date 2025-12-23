# Creating a Private Subnet

**Project Link:** https://learn.nextwork.org/projects/aws-networks-private  
**Author:** Shane Brown  

---

## Overview

This project focuses on designing a secure network architecture in AWS by creating and configuring private subnets within an Amazon Virtual Private Cloud (VPC). The goal is to understand how private subnets isolate sensitive resources and how routing and network controls enforce that isolation.

This project builds on previous VPC and security concepts and demonstrates how real-world cloud environments separate public-facing resources from internal systems. :contentReference[oaicite:0]{index=0}

---

## What I built

I designed a VPC network that includes both public and private subnets, with a focus on securing the private subnet. This included:

- Creating a private subnet with its own CIDR block  
- Associating the private subnet with a dedicated route table  
- Ensuring no direct route to an Internet Gateway  
- Creating and attaching a custom Network ACL for subnet-level control  
- Allowing only internal VPC traffic to and from the private subnet  

This setup keeps sensitive resources isolated from direct internet access while still allowing controlled internal communication. :contentReference[oaicite:1]{index=1}

---

## Key concepts learned

- Differences between public and private subnets  
- How CIDR blocks must be unique within a VPC  
- How route tables control subnet traffic flow  
- Why private subnets use the main or restricted route tables  
- How Network ACLs add an additional security layer at the subnet level  
- How private subnets reduce attack surface and improve security posture :contentReference[oaicite:2]{index=2}

---

## Private vs Public Subnets

- **Public Subnets**
  - Have a route to an Internet Gateway  
  - Host resources that must be accessible from the internet  

- **Private Subnets**
  - Have no direct route to the Internet Gateway  
  - Host internal or sensitive resources such as databases or backend services  
  - Can access the internet only through controlled mechanisms like NAT  

This separation is a core best practice for secure cloud architectures. :contentReference[oaicite:3]{index=3}

---

## Why this project matters

Private subnets are essential for protecting sensitive systems in the cloud. By isolating internal resources and carefully controlling traffic with route tables and network ACLs, organizations can significantly reduce risk while maintaining flexibility and scalability.

This project demonstrates how AWS networking components work together to enforce security by design rather than by accident. :contentReference[oaicite:4]{index=4}

---

## Documentation

📄 **Full project documentation:**  
[documentation.md](./documentation.md)

This file includes detailed explanations, configurations, reflections, and diagrams from completing the project.

---

## Credits

Built as part of the **NextWork** AWS networking learning series.
