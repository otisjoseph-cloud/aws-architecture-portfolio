# Secure VPC Architecture

![Secure VPC Architecture](diagrams/architecture.png)

If Availability Zone A fails:

Resources in AZ B remain operational.

Load Balancer continues routing to healthy instances.

Private subnets in AZ B retain outbound internet access via NAT-B.

Database remains available via Multi-AZ deployment.

