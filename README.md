# Secure VPC Architecture on AWS

This repository presents a secure, multi-AZ AWS VPC reference architecture designed for high availability, network segmentation, controlled administrative access, and cost-aware cloud design.

The goal of this project is to demonstrate cloud architecture judgment through documented design decisions, diagrams, security considerations, operational monitoring notes, service tradeoffs, failure scenarios, and improvement paths.

## Project Scope

This project focuses on a secure VPC architecture with:

- Public subnets for internet-facing components
- Private application subnets for compute resources
- Private database subnets with no direct internet route
- NAT Gateways in each Availability Zone for resilient outbound access
- Application Load Balancer traffic flow
- AWS Systems Manager Session Manager instead of bastion-host SSH access
- Security group boundaries between public, application, and database tiers
- Route table design and subnet-level traffic control
- Monitoring and operational visibility considerations
- Availability Zone failure scenario review

## Included

- Architecture diagram
- Design overview
- Design decisions
- Security considerations
- Cost considerations
- Tradeoff analysis
- Failure scenario review
- Operational monitoring notes
- Route table summary
- Security group model
- IAM and access model
- Interview explanation

## Repository Structure

- `01-secure-vpc/` - Secure multi-AZ VPC architecture project

## Focus Areas

- AWS VPC architecture fundamentals
- Secure network segmentation
- High availability and failure planning
- Cost-aware architecture decisions
- Monitoring and operational visibility
- IAM and administrative access controls
- Healthcare and regulated-environment design thinking

## Author

Otis Joseph  
AWS Certified Solutions Architect - Associate  
AWS Certified Cloud Practitioner  
AWS Certified AI Practitioner