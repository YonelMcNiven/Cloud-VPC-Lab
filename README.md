# Cloud-VPC-Lab
Built and validated a custom AWS VPC (10.0.0.0/16) with segmented public/private /24 subnets, IGW routing, Security Groups, and EC2. Deployed Apache, secured SSH with /32 and agent forwarding, and tested TCP, HTTP/HTTPS, ICMP, private isolation, and east-west connectivity.
# AWS VPC Architecture & Network Segmentation Lab

## Overview
Built and validated a custom AWS VPC using a `10.0.0.0/16` CIDR block with separate public and private `/24` subnets. The lab focused on subnet segmentation, routing, security groups, EC2 deployment, SSH access, and connectivity testing.
## Architecture
The environment consisted of:

- One custom VPC
- One public subnet
- One private subnet
- One Internet Gateway
- Separate public and private route tables
- A public EC2 web server
- A private EC2 instance with no direct internet access
- ![VPC Architecture](evidence/01-vpc-resource-map.png)evidence/Screenshot 2026-10-01 171014.png
## Network Design
The VPC used the following addressing scheme:

- VPC: `10.0.0.0/16`
- Public subnet: `10.0.1.0/24`
- Private subnet: `10.0.2.0/24`

The public and private subnets were placed in separate Availability Zones to demonstrate basic network segmentation.
## Routing
The public route table contained:

- `10.0.0.0/16` → local
- `0.0.0.0/0` → Internet Gateway

The private route table contained only:

- `10.0.0.0/16` → local

Because the private subnet had no NAT Gateway or default internet route, the private EC2 instance remained isolated from the public internet.

![Route Tables](evidence/03-route-tables.png)
## Security Groups
The public EC2 security group permitted:

- TCP 80 for HTTP
- TCP 443 for HTTPS
- TCP 22 for SSH restricted to the administrator's current public IP using `/32`

Outbound traffic was permitted for connectivity testing.
## Implementation
An Amazon Linux EC2 instance was deployed in the public subnet and configured with Apache HTTP Server.

A second EC2 instance was deployed in the private subnet without a public IP address.

SSH agent forwarding was used to connect:

`Local Windows workstation → Public EC2 → Private EC2`

This allowed administration of the private host without storing the SSH private key on the bastion host.
## Validation Tests
The environment was tested using multiple methods:

- HTTP access to the public web server
- HTTPS connectivity
- ICMP testing
- SSH connectivity
- TCP port testing
- Private-to-public subnet communication
- Internet connectivity testing from the private EC2 instance
- Linux socket and process inspection using `ss -tulpn`

The private instance was able to communicate internally but could not directly access the internet, confirming that the routing design worked as intended.

![Connectivity Validation](evidence/05-public-ec2-http-test.png)
## Troubleshooting
Several issues were encountered and resolved during the lab:

- Internet Gateway route configuration initially failed because the gateway had not yet been attached to the VPC.
- SSH agent forwarding initially failed because the authentication agent was not loaded correctly.
- A change in the administrator's home public IP caused SSH connectivity to fail until the Security Group `/32` rule was updated.

These issues reinforced the relationship between routing, security groups, authentication, and endpoint connectivity.

## Evidence
Screenshots in the `/evidence` directory document:

- VPC architecture
- Subnet configuration
- Route tables
- Security Group rules
- EC2 placement
- SSH agent forwarding
- HTTP connectivity
- Private subnet isolation
- Internal connectivity
- Linux service and socket inspection

See the full evidence set in the [`/evidence`](./evidence) directory.
## Evidence
- AWS VPC design
- CIDR planning and subnetting
- Public/private subnet segmentation
- Internet Gateway configuration
- Route table administration
- Security Group configuration
- EC2 networking
- Linux administration
- SSH agent forwarding
- TCP/IP troubleshooting
- HTTP/HTTPS validation
- Network isolation testing
- Linux socket inspection
- Cloud troubleshooting

## Skills Demonstrated
- AWS VPC design
- CIDR planning and subnetting
- Public/private subnet segmentation
- Internet Gateway configuration
- Route table administration
- Security Group configuration
- EC2 networking
- Linux administration
- SSH agent forwarding
- TCP/IP troubleshooting
- HTTP/HTTPS validation
- Network isolation testing
- Linux socket inspection
- Cloud troubleshooting

## Cleanup
After validation, the EC2 instances and associated lab resources were terminated or deleted to avoid unnecessary AWS charges.
## Lessons Learned
This lab demonstrated that successful cloud networking depends on multiple layers working together. Subnets alone do not make a resource public or private; routing, public IP assignment, Internet Gateway connectivity, and Security Group policy all contribute to the final network behavior.

The troubleshooting process also reinforced the importance of validating connectivity layer by layer rather than assuming a failure is caused by a single service.<img width="1910" height="994" alt="VPC SC S 1" src="https://github.com/user-attachments/assets/08224ef0-5e44-43ae-a026-338f9ff72b06" />
<img width="1898" height="898" alt="Screenshot 2026-10-01 171149" src="https://github.com/user-attachments/assets/1b6ae888-fd4a-4342-ac53-78224b2a3b83" />
<img width="1855" height="955" alt="Screenshot 2026-10-01 171014" src="https://github.com/user-attachments/assets/e7b74e99-bca7-4d0d-aba0-6fc09c5a5506" />

