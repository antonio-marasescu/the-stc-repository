# Elastic Load Balancing (ELB) & Auto-Scalling Groups (ASG)

**Scalability** = application can handle greater loads by adapting

There are 2 types of scalability:
- Vertical (increasing the size of the instance)
- Horizontal (increasing the number of instances)

**High Availability** = running your application in at least 2 Availability Zones
**Elasticity** = scale per use (if low-use down-scale, if highly-used up-scale)

**Load Balancers** = servers that forward internet traffic to multiple servers downstream.
They are used for:
- Spreading load across multiple instances
- Exposes a single-point of access (DNS) to your application
- Seamlessly handle failure of downstream instancs
- Do regular healthchecks
- Provide SSL
- High Availability

## Elastic Load Balancer (ELB)

Represents a managed load balancer (aws guarantees: it will be working, upgrades, maintenance, high availability, configuration knobs).
It has 3 types:
- Application Load Balancer (ALB) (HTTP(s)) - works on layer 7 (OSI model)
- Network Load Balancer (NLB) (ultra-high performance, allows for TCP) - works on layer 4
- Classic Load Balancer (slowly retiring) - works on layer 7 and 4
