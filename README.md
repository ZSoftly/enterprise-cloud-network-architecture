# Enterprise Cloud Network Architecture

A comprehensive guide for designing and implementing enterprise-grade cloud network architecture focusing on AWS infrastructure.

## Author
Ditah Kumbong

*This documentation was initially generated using Claude AI and has been thoroughly reviewed, adjusted, and enhanced based on my professional experience in cloud infrastructure and network design.*

## Network Design Overview

### Private IP Range Allocation (10.x.x.x)
- Primary Region (Singapore): `10.0.0.0/8`
- Europe Region: `10.10.0.0/8`
- US East Region: `10.20.0.0/8`

### Environment Segmentation (per region)
```
Development:  10.x.0.0/16   to 10.x.49.0/16
Staging:      10.x.50.0/16  to 10.x.99.0/16
Production:   10.x.100.0/16 to 10.x.199.0/16
Future Use:   10.x.200.0/16 to 10.x.255.0/16

where x represents region (0 for SG, 10 for EU, 20 for US)
```

### Tier Segmentation (per environment)
```
Web Tier:     10.x.y.1.0/24, 10.x.y.2.0/24
App Tier:     10.x.y.3.0/24, 10.x.y.4.0/24
Database:     10.x.y.5.0/24, 10.x.y.6.0/24
Management:   10.x.y.7.0/24, 10.x.y.8.0/24
Reserved:     10.x.y.9.0/24, 10.x.y.10.0/24

where:
x = region number (0, 10, 20)
y = environment number (0-49 dev, 50-99 staging, 100-199 prod)
```

## Design Principles

### 1. IP Address Pattern
Example: `10.0.100.5.25` breakdown:
- `10.0`: Singapore Region
- `100`: Production Environment
- `5`: Database Tier
- `25`: Host number

### 2. Network Security Design
- Public subnets: Web tier with internet access
- Private subnets: App and DB tiers
- Protected subnets: Management and monitoring
- Each tier has dedicated security groups and NACLs

### 3. High Availability Design
- Minimum two AZs per region
- Active-Active configuration for web/app tiers
- Active-Standby for database tier
- Cross-AZ load balancing

### 4. Resource Distribution
Per Availability Zone:
- Web servers in public subnets
- Application servers in private subnets
- Database instances in protected subnets
- NAT Gateways in public subnets
- Load Balancers spanning all AZs

## Network Architecture Diagrams

1. [Global Network Design](diagrams/global-network.md)
   - Enterprise-wide network structure
   - Regional connectivity
   - Transit Gateway design

2. [Regional Network Design](diagrams/regional-design.md)
   - VPC structure
   - Environment segmentation
   - CIDR allocation

3. [Availability Zones Design](diagrams/availability-zones.md)
   - Multi-AZ layout
   - Subnet distribution
   - HA components

## Implementation Best Practices

### 1. VPC Components
- One Internet Gateway per VPC
- NAT Gateway per AZ
- VPC Endpoints for AWS services
- Transit Gateway for inter-VPC routing

### 2. Security Implementation
```
Security Group Chain:

Web Tier SG:
- Inbound: 80/443 from Internet
- Outbound: To App Tier ports

App Tier SG:
- Inbound: From Web Tier only
- Outbound: To DB Tier ports

DB Tier SG:
- Inbound: From App Tier only
- Outbound: Updates only
```

### 3. Connectivity
- VPC Peering for direct connections
- Transit Gateway for hub-and-spoke
- Direct Connect for on-premises
- Site-to-Site VPN for backup

### 4. Monitoring
- VPC Flow Logs enabled
- CloudWatch metrics
- Network monitoring
- Security monitoring

## Growth Considerations

### 1. Subnet Expansion
```
Initial Subnets (Singapore Production):
Web: 10.0.100.1.0/24 (AZ1), 10.0.100.2.0/24 (AZ2)
App: 10.0.100.3.0/24 (AZ1), 10.0.100.4.0/24 (AZ2)
DB:  10.0.100.5.0/24 (AZ1), 10.0.100.6.0/24 (AZ2)

Growth Subnets:
Web: 10.0.100.11.0/24 (AZ1), 10.0.100.12.0/24 (AZ2)
App: 10.0.100.13.0/24 (AZ1), 10.0.100.14.0/24 (AZ2)
DB:  10.0.100.15.0/24 (AZ1), 10.0.100.16.0/24 (AZ2)
```

### 2. Regional Expansion
- Keep consistent naming
- Use standard CIDR blocks
- Maintain security patterns
- Replicate monitoring setup

## Quick Start Guide

1. Start with primary region setup
2. Implement basic networking components
3. Set up security groups and NACLs
4. Configure monitoring and logging
5. Expand to additional regions as needed

## License
MIT
