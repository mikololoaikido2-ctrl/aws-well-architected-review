# aws-well-architected-review
Well-Architected Framework review of a sample architecture — findings and recommendations across security, reliability, and cost pillars, with before/after diagrams.
# AWS Well-Architected Framework Review — GrowthCo Web Application

A Well-Architected Framework review of a sample SaaS startup's AWS architecture, identifying risks across the Reliability, Security, Cost Optimization, and Operational Excellence pillars, with a redesigned architecture addressing each finding.

## Overview

This project simulates a Well-Architected review for **GrowthCo**, a fictional small SaaS startup, using AWS's official Well-Architected Tool. The goal was to assess a common "early-stage startup" architecture — functional, but built without redundancy, security hardening, or cost efficiency in mind — and produce the kind of findings and recommendations a client would receive from a real audit.

## The Scenario

GrowthCo's current architecture:
- A single EC2 instance running the web application (no redundancy)
- A single-AZ RDS database with no automated backups configured
- An S3 bucket for user uploads, publicly accessible with no encryption
- No load balancer or auto-scaling
- Everything deployed in a single Availability Zone

## Methodology

1. Defined the workload in the AWS Well-Architected Tool
2. Answered the standard questionnaire across four pillars: Reliability, Security, Cost Optimization, and Operational Excellence
3. Reviewed the tool's auto-generated risk findings
4. Mapped each finding to a specific, actionable recommendation
5. Redesigned the architecture to address every high-risk item

## Current Architecture

![Current Architecture](diagrams/current-architecture.png)

Single EC2 instance, single-AZ database, public S3 bucket — no failover, no encryption, no scaling.

## Findings

| Pillar | Finding | Risk | Recommendation |
|---|---|---|---|
| Reliability | Single-AZ RDS, no automated backups | High | Enable Multi-AZ deployment + automated daily backups |
| Security | S3 bucket publicly accessible, no encryption | High | Enable Block Public Access + default SSE-S3 encryption |
| Reliability | Single EC2 instance, no failover | High | Deploy behind an Application Load Balancer with a minimum of 2 instances across AZs |
| Cost Optimization | Fixed EC2 capacity, no auto-scaling | Medium | Implement an Auto Scaling Group sized to actual traffic patterns |
| Operational Excellence | No monitoring or alerting configured | Medium | Add CloudWatch alarms for CPU, error rates, and database connections |
| Security | Database not in a private subnet | Medium | Move RDS into a private subnet, restrict access via security groups |

## Recommended Architecture

![Recommended Architecture](diagrams/recommended-architecture.png)

Load-balanced EC2 instances across two Availability Zones, Multi-AZ RDS with automated backups, private S3 bucket with encryption and versioning, and CloudWatch monitoring throughout.

## Outcome

Addressing the high-risk findings would move GrowthCo from a single point of failure on both compute and database layers to a fault-tolerant architecture, while closing a public data-exposure risk on the S3 bucket. The Auto Scaling and monitoring recommendations additionally position the workload to scale cost-efficiently as traffic grows, rather than over-provisioning a fixed instance size.

## Tools Used

- AWS Well-Architected Tool
- Amazon EC2, RDS, S3, CloudWatch, Application Load Balancer, Auto Scaling
- draw.io (architecture diagrams)

## Notes

This review is based on a representative sample scenario rather than a live client environment, built to demonstrate Well-Architected review methodology and findings structure.
