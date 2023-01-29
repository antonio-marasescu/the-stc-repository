# Global Infrastructure 

A **global** application is an application deployed in multiple geographic locations (on AWS this could Regions or Edge Locations).
This will offer you decreased latency (time for a network packet to reach a server) and disaster recovery (if aws region goes to down represent a failover for you application to still work).

On AWS this is made by:
- **Regions**: for deploying applications and infrastructure
- **Availability Zones** (inside of a region): made of multiple data centers
- **Edge Location** (Points of Presence): for fast content delivery

## Summary

### Global Applications in AWS - Summary
- Global DNS: Route 53
  - Great to route users to the closest deployment with least latency
  - Great for disaster recovery strategies
- Global Content Delivery Network (CDN): CloudFront
  - Replicate part of your application to AWS Edge Locations – decrease latency
  - Cache common requests – improved user experience and decreased latency
- S3 Transfer Acceleration
  - Accelerate global uploads & downloads into Amazon S3
- AWS Global Accelerator
  - Improve global application availability and performance using the AWS global network
- AWS Outposts
  - Deploy Outposts Racks in your own Data Centers to extend AWS services
- AWS WaveLength
  - Brings AWS services to the edge of the 5G networks
  - Ultra-low latency applications
- AWS Local Zones
  - Bring AWS resources (compute, database, storage, …) closer to your users
  -  Good for latency-sensitive applications

## Global Application in AWS

### Global DNS: Route 53

A managed DNS (Domain Name System = a collection of rules and records which help clients understand how to reach a server through URLs) offered by AWS.
There are several **Routing Policies** offered:
- Simple Routing Policy (no health checks)
- Weighted Routing Policy (route based on a weight on how much traffic should go somewhere)
- Latency Routing Policy (based on user location)
- Failover Routing Policy (with a healthcheck)

### Global Content Delivery Network (CDN): CloudFront

A content delivery network (CDN) offered by AWS which will offer an increased read performance with content cached at the edge. It has included a DDOS protection due to its integration with Shield and AWS Web Application Firewall.
Used in corellation with S3 for distributing files (or static websites) and can be used as an ingress (for uploading files).

#### Cloudfront vs S3 Cross Region Replication

Basically Cloudfront is great for static content with limited setup vs Cross Region replication which is great for dynamic content and which requires some setup.

### S3 Transfer Acceleration

A service to increase transfer speed of file upload by transfering to an AWS edge location which will later forward it to a S3 bucket in the target region.

### AWS Global Accelerator



### AWS Outposts
### AWS WaveLength
### AWS Local Zones
