# Virtual Private Cloud (VPC)

## Summary 

- **VPC**: Virtual Private Cloud
- **Subnets**:Tied to an AZ, network partition of the VPC
- **Internet Gateway**: at the VPC level, provide Internet Access
- **NAT Gateway / Instances**: give internet access to private subnets
- **NACL**: Stateless, subnet rules for inbound and outbound
- **Security Groups**: Stateful, operate at the EC2 instance level or ENI
- **VPC Peering**: Connect two VPC with non overlapping IP ranges, nontransitive
- **VPC Endpoints**: Provide private access to AWS Services within VPC
- **PrivateLink**: Privately connect to a service in a 3rd party VPC
- **VPC Flow Logs**: network traffic logs
- **Site to Site VPN**: VPN over public internet between on-premises DCand AWS
- **Client VPN**: OpenVPN connection from your computer into your VPC
- **Direct Connect**: direct private connection to AWS
- **Transit Gateway**: Connect thousands of VPC and on-premises networks together
