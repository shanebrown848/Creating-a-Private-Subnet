<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Creating a Private Subnet

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-private)

**Author:** Shane Brown  
**Email:** shanebrown848@gmail.com

---

## Creating a Private Subnet

![Image](http://learn.nextwork.org/encouraged_yellow_silly_yeti/uploads/aws-networks-private_afe1fdbd)

---

## Introducing Today's Project!

### What is Amazon VPC?

Amazon VPC (Virtual Private Cloud) lets me build my own isolated network inside AWS where I control how resources communicate. It’s useful because I can split my network into public and private subnets — public subnets allow internet access for things like web servers, while private subnets keep sensitive resources hidden. Each subnet has unique IP ranges to avoid conflicts. By default, private subnets use the main route table, which only allows internal VPC traffic. For public subnets, I create a custom route table with a route to the internet gateway. I also set up dedicated network ACLs to control which traffic can enter or leave my subnets, adding extra security. VPC gives me full control over security, traffic flow, and resource isolation in the cloud.

### How I used Amazon VPC in this project

In today’s project, I used Amazon VPC to design a secure network with both public and private subnets. I created public subnets for resources that need internet access and private subnets for internal resources that should stay isolated. I set up custom route tables for the public subnets with routes to the internet gateway, while private subnets used the main route table for internal-only traffic. I also created dedicated network ACLs for my private subnets to control inbound and outbound traffic, allowing only trusted internal communication. This setup gave me full control over how my resources communicate while keeping everything secure and organized.

### One thing I didn't expect in this project was...

One thing I didn’t expect in this project was how important the route tables and network ACLs would be for controlling traffic flow. At first, I thought creating subnets was enough, but I quickly realized that without proper routes and ACL configurations, my resources couldn’t communicate the way I wanted. It really showed me how much control Amazon VPC gives, but also how careful you have to be when setting everything up.

### This project took me...

Now this project took me about 45 mins, because I already had my resources still available from the last couple of projects. I didn't have to redo any steps.

---

## Private vs Public Subnets

The difference between public and private subnets is that public subnets are accessible directly from the internet, while private subnets are isolated from direct internet access.

Here’s a bit more detail:

Public Subnet:

Contains resources (like web servers) that need to be reachable from the internet.

Has a route to an internet gateway.

Resources in a public subnet typically have public IP addresses or Elastic IPs to allow inbound/outbound internet traffic.

Private Subnet:

Contains resources (like databases, internal services) that should not be exposed to the internet.

Has no direct route to an internet gateway.

Resources usually communicate externally via a NAT Gateway or NAT Instance, which allows them to initiate outbound internet connections (for updates, patches, etc.) without exposing them to inbound internet traffic.

Having private subnets are useful because they help protect sensitive resources by isolating them from direct internet exposure, reducing the attack surface and enhancing security.

More specifically:

Security:

Keeps critical systems (like databases, internal apps, or backend services) hidden from the public internet.

Prevents unauthorized inbound traffic directly from outside sources.

Compliance & Best Practices:

Many security standards (PCI, HIPAA, etc.) recommend or require sensitive data to reside in private subnets.

Follows the principle of least privilege — only necessary systems are exposed.

Control:

Outbound internet access can still be controlled via NAT Gateways, so systems can update or access external resources safely.

Internal traffic between services stays within the private network.

Cost Optimization:

Private subnets often don’t require public IP addresses, which can simplify IP management and save costs depending on cloud provider models.

My private and public subnets cannot have the same IP address range (CIDR block) within the same VPC.

Here’s why:

Each subnet within a VPC must have a unique CIDR block that doesn’t overlap with any other subnet in that VPC.

This allows the network to route traffic correctly between subnets.

Even though both subnets exist within the same VPC and availability zone, they need distinct IP ranges to avoid conflicts and ensure proper routing.

For example:
Public Subnet: 10.0.1.0/24
Private Subnet: 10.0.2.0/24
They share the same VPC (10.0.0.0/16), but have different subnet ranges.

![Image](http://learn.nextwork.org/encouraged_yellow_silly_yeti/uploads/aws-networks-private_afe1fdbd)

---

## A dedicated route table

By default, my private subnet is associated with the main route table of the VPC. This main route table doesn't have a route to the internet gateway unless I specifically add one. That’s what keeps my private subnet isolated and secure by default, no direct internet access unless I configure NAT or other specific routes. Basically, out of the box, AWS keeps anything in my private subnet tucked away from the outside world, and it's up to me if I want to give it controlled access later.

I had to set up a new route table because I needed to control how traffic flows for my specific subnet. For example, when I create a public subnet, I need a route table that includes a route to the internet gateway so that resources in that subnet can communicate with the internet. The default main route table doesn’t have that by default. By creating a separate route table, I can assign it to my public subnet, define the exact routes I want, and keep my private subnets safely isolated without accidentally exposing them. It’s all about having that fine-tuned control over who talks to who in my VPC.

My private subnet's dedicated route table only has one inbound and one outbound rule that allows local traffic within the VPC using the "local" route. This means resources inside the private subnet can communicate with other subnets and resources within the same VPC. By default, there’s no route to the internet gateway, so it doesn’t allow direct internet access. If I want the private subnet to reach the internet (for updates or downloads), I would need to add a route to a NAT gateway or NAT instance, but initially, it's just limited to internal VPC traffic.

![Image](http://learn.nextwork.org/encouraged_yellow_silly_yeti/uploads/aws-networks-private_b4b904b5)

---

## A new network ACL

By default, my private subnet is associated with the VPC’s default Network ACL (NACL). This default NACL is configured to allow all inbound and outbound traffic, which means it’s open by default. However, even though the NACL allows all traffic, the private subnet still doesn’t have internet access because the route table controls where the traffic can actually go. If I want tighter security, I can create custom NACLs to explicitly allow or deny specific types of traffic at the subnet level.

I set up a dedicated network ACL for my private subnet because I wanted to add an extra layer of security by controlling which specific types of traffic are allowed in and out of the subnet. While security groups control traffic at the instance level, the NACL works at the subnet level, allowing me to define stateless rules that apply to all resources inside. For example, I might block certain IP ranges, restrict specific ports, or limit traffic to only internal communication. This helps me lock things down even more, making sure that only the exact traffic I want is allowed to flow in or out of my private subnet.

My new network ACL has two simple rules, one inbound and one outbound. The inbound rule allows traffic from the internal VPC CIDR range so that resources within my VPC can communicate with my private subnet. The outbound rule allows traffic to the same internal VPC range, ensuring that instances in my private subnet can respond to requests and interact with other subnets or services inside the VPC. There are no rules allowing traffic to or from the internet directly, keeping the subnet fully isolated except for trusted internal communication.

![Image](http://learn.nextwork.org/encouraged_yellow_silly_yeti/uploads/aws-networks-private_1ed2cb07)

---

---
