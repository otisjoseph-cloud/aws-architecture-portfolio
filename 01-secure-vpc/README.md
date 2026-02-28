# Secure VPC Architecture

![Secure VPC Architecture](diagrams/architecture.png)

## Failure Scenario: AZ-A Outage

If Availability Zone A fails:
- Resources in AZ B remain operational.
- Load Balancer routes to healthy targets.
- Private subnets in AZ B retain outbound access via NAT-B.
- Database remains available via Multi-AZ deployment.


