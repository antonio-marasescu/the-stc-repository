# Elastic Compute Cloud (EC2)

It basically offers a server (IaS) with the options to customize:
- OS: Linux, Windows, MacOS
- CPU
- RAM
- Storage Space
  - Network Attached: EBS & EFS
  - Hardware: EC2 Instance Store
- Network Card: speed of the card, public IP address
- Firewall Rules: security group
- Bootstrap script: EC2 User Data

## Instance Types

1. General Purpose: great for a diversity of workloads such as web servers (has balance between compute, memory, networking)
2. Compute Optimized: great for compute-intensive tasks that require high performance processors (batch processing workloads, media transcoding, high performance web servers, dedicated gaming servers)
3. Memory Optimized: fast performance for workloads that process large data sets in memory (high performance relational/non-relation databases, distributed web scale cache stores, real-time processing of big unstructured data)
4. Storage Optimized: great for storage intensive tasks that require high, sequential read and write access to large data sets on local storage (online transactions processing systems, relational & nosql databases, data warehousing)

## Security Groups

- They control how traffic is allowed in/out of the EC2 instances, acting as a sort of "firewall"
- The security groups only contain **ALLOW** rules
  - all inbound traffic is blocked by default
  - all outbound traffic is authorized by default
- Security group rules can reference by IP or by security group.
- Can be attached to multiple instances
- Locked down to a region/VPC combination
- Lives "outside" of the EC2, if traffic is blocked => EC2 instance won't see it

### Classic Ports
- 22 = SSH (Secure Shell)
- 21 = FTP (File Transfer Protocol)
- 22 = SFTP (Secure File Transfer Protocol)
- 80 = HTTP (access unsecured websites)
- 443 = HTTPS (access secured websites)
- 3389 = RDP (Remote Desktop Protocol) - log into a windows instance

## Instance Purchasing Options

- On-Demand Instances - short workload, predictable pricing, pay by second
- Reserved (1 & 3 years): reserve specific instance attributes (type, region, tenancy, os)
  - Reserved Instances - long workloads
  - Convertible Reserved Instance - long workloads with flexible instance
- Saving Plans (1 & 3 years) - commitment to an amount of usage, long workload
- Spot Instances - short workloads, cheap, can lose instances (less reliable). You can "lose" at any point of time if your max price is less that the current spot price.
- Dedicated hosts - book an entire physical server, control instance placement
- Capacity Reservations - reserve capacity in a specific AZ for any duration
