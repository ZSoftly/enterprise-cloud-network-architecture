# Regional Network Design

```mermaid
graph LR
    subgraph "Production Environment Detail"
        PROD[Production VPC<br>10.0.100.0/16] --> WEB[Web Tier<br>10.0.100.1.0/24,<br>10.0.100.2.0/24]
        PROD --> APP[App Tier<br>10.0.100.3.0/24,<br>10.0.100.4.0/24]
        PROD --> DB[DB Tier<br>10.0.100.5.0/24,<br>10.0.100.6.0/24]
        PROD --> MGT[Management<br>10.0.100.7.0/24,<br>10.0.100.8.0/24]
    end

    subgraph "Network Components"
        WEB --> IGW[Internet Gateway]
        WEB --> NAT[NAT Gateway]
        APP --> VPE1[VPC Endpoints]
        DB --> VPE2[VPC Endpoints]
        PROD --> TGW[Transit Gateway<br>Regional Resource]
    end

    subgraph "Component Notes"
        NAT -->|"Placed in Public Subnet"| WEB
        VPE1 -->|"Specific to App Tier"| APP
        VPE2 -->|"Specific to DB Tier"| DB
        TGW -->|"Regional Service"| PROD
    end
```

## Design Pattern
1. NAT Gateway in public subnet (Web Tier)
2. VPC Endpoints per private tier as needed
3. Transit Gateway as regional service
4. Clear separation of network components

Author: Ditah Kumbong
