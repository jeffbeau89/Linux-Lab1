# Lab Architecture

## Network Design

**VPC:** lab-vpc (10.0.0.0/16)  
**Subnet:** lab-public-subnet (10.0.1.0/24)  
**Internet Gateway:** lab-igw

## Security Group Design

| Node | Security Group | Allowed Inbound |
|---|---|---|
| control01 | sg-control | SSH from admin IP only |
| web01 | sg-web | SSH from admin IP, HTTP/HTTPS public |
| db01 | sg-db | SSH from admin IP, 3306 from web tier only |

## Design Decisions

**Why a custom VPC?**  
Default VPCs have permissive configurations unsuitable for hardened 
environments. Custom VPC gives explicit control over routing, subnetting, 
and network boundaries.

**Why is db01 not publicly accessible?**  
Database servers should never be exposed to the internet directly. 
Network segmentation ensures only the application tier can reach the 
data tier — a core principle of defense in depth.
