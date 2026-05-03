# Secure VPC Architecture

## Overview

This project presents a secure, multi-AZ VPC architecture designed for high availability, network segmentation, and controlled administrative access. The design separates public, private application, and private database resources across two Availability Zones to improve resilience and reduce attack surface.

## Architecture Diagram

![Secure VPC Architecture](diagrams/architecture.png)

## Design Summary

The architecture uses:

- One VPC spanning two Availability Zones
- Public subnets for internet-facing components
- Private application subnets for compute resources
- Private database subnets with no direct internet route
- NAT Gateways in each Availability Zone for resilient outbound access
- AWS Systems Manager Session Manager instead of a bastion host for administrative access

## Design Decisions

### 1. Multi-AZ Deployment

Resources are distributed across two Availability Zones to reduce the impact of a single AZ failure and improve availability.

### 2. Public, Private, and Database Subnet Separation

The architecture isolates internet-facing resources from application and database resources. This reduces exposure and supports least-privilege network design.

### 3. NAT Gateway in Each Availability Zone

Each private application subnet uses a NAT Gateway in the same Availability Zone. This avoids dependence on a single NAT Gateway and reduces cross-AZ dependency during failure scenarios.

### 4. No Bastion Host

Instead of exposing SSH access through a bastion host, the design uses AWS Systems Manager Session Manager. This reduces attack surface, avoids managing inbound SSH rules, and improves administrative security.

### 5. Private Database Subnets with No Internet Route

Database resources are placed in private subnets with no direct internet access. This helps protect sensitive data and aligns with secure application architecture patterns.

## Security Considerations

- Application and database resources are separated into private subnets
- Database subnets have no route to the internet gateway
- Administrative access is handled through Systems Manager Session Manager instead of SSH bastion access
- Security groups should allow only required traffic between tiers, such as application-to-database communication
- Public exposure should be limited to only required internet-facing components
- Long-lived shared credentials should be avoided where possible
- Administrative access should follow least-privilege IAM permissions

## Operational Monitoring

This design should include CloudWatch metrics and alarms for NAT Gateway data processing, EC2 instance health, load balancer target health, database CPU/storage, and application availability.

Logs should be centralized where possible to support troubleshooting, audit review, and operational visibility.

Recommended monitoring areas:

- NAT Gateway bytes processed and error patterns
- EC2 instance status checks
- Load balancer target health
- Database CPU, storage, connections, and failover events
- Application availability and latency
- Security-related access patterns and failed access attempts

## Route Table Summary

Public subnets route internet-bound traffic to the Internet Gateway.

Private application subnets route outbound internet traffic through the NAT Gateway in the same Availability Zone.

Private database subnets do not route directly to the internet and should only allow required traffic from the application tier.

## Security Group Model

Internet-facing components should only allow required inbound traffic such as HTTPS.

Application resources should only accept traffic from the load balancer or approved internal sources.

Database resources should only accept required database traffic from the application security group.

Administrative access should use Systems Manager Session Manager rather than public SSH access.

## IAM and Access Model

Systems Manager Session Manager requires appropriate IAM roles, SSM agent availability, and private or outbound network access to AWS Systems Manager endpoints.

Administrative access should follow least privilege and avoid long-lived shared credentials.

Recommended access model:

- Use IAM roles for AWS service access
- Avoid public SSH access
- Use Session Manager for administrative access
- Restrict permissions based on job function
- Review and rotate access regularly
- Log administrative sessions where appropriate

## Cost Considerations

- Using a NAT Gateway in each Availability Zone increases cost compared to a single NAT Gateway design
- The additional NAT Gateway cost is justified by improved resilience and reduced dependence on cross-AZ traffic
- Systems Manager Session Manager reduces the need to run and maintain a bastion host, which can lower operational overhead
- Multi-AZ design increases infrastructure cost but improves uptime and fault tolerance
- VPC endpoints could reduce NAT Gateway data processing costs for private AWS service access in future iterations

## Tradeoff Analysis

### Benefits

- Improved availability through multi-AZ deployment
- Stronger security through subnet isolation and no bastion host
- Better operational access model using Systems Manager
- Reduced risk of full service interruption during a single AZ outage
- Clearer separation between public, application, and database tiers

### Tradeoffs

- Higher cost due to one NAT Gateway per Availability Zone
- More complexity than a simple single-AZ architecture
- Session Manager requires proper IAM role configuration and setup
- Multi-tier subnet design may be more difficult for beginners to configure and troubleshoot
- Additional monitoring and documentation are needed to support operational visibility

## Failure Scenario: AZ-A Outage

If Availability Zone A fails:

- Resources in AZ B remain operational
- Traffic can continue to healthy resources in AZ B
- Private subnets in AZ B retain outbound access through NAT-B
- Database availability can continue if deployed with a Multi-AZ database configuration

## Failure Scenario Notes

- Existing sessions connected to resources in AZ A may be interrupted
- Recovery depends on workload design, including stateless application behavior and proper load balancing
- Failover can increase load on remaining resources in AZ B
- Operating in degraded mode may temporarily reduce capacity until recovery is complete
- Application recovery depends on health checks, scaling policies, and database failover configuration

## Improvements for Future Iterations

Future enhancements could include:

- Network ACL discussion
- VPC endpoints for private AWS service access
- More detailed database failover notes
- Example CloudWatch alarm thresholds
- Estimated monthly cost comparison between single NAT Gateway and multi-AZ NAT Gateway designs

## Interview Explanation

This architecture demonstrates how I think through AWS network design for a regulated or healthcare-adjacent workload. I separated public, private application, and database resources, used multiple Availability Zones for resilience, avoided bastion-host SSH exposure by using Session Manager, and documented the cost tradeoff of using NAT Gateways in each Availability Zone.

The main tradeoff is higher cost and complexity in exchange for stronger availability, security, and operational maintainability.

## Key Takeaways

This architecture demonstrates how AWS networking can be designed for security, resilience, and maintainability. The design prioritizes controlled access, private resource placement, and reduced single points of failure, while accepting higher cost and complexity as tradeoffs for stronger availability and security.
