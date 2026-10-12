# IBM MQ Clustering Lab

A beginner-friendly, hands-on learning repository to understand IBM MQ clustering through practical examples, configuration exercises, and troubleshooting scenarios.

## Learning Objectives

* Understand IBM MQ clustering architecture and core concepts.
* Learn the roles of Full Repository and Partial Repository queue managers.
* Configure cluster sender and cluster receiver channels.
* Create and configure cluster queues.
* Understand message routing and workload balancing.
* Troubleshoot common clustering issues.
* Learn clustering configuration and operational best practices.

## Lab Architecture

The initial lab uses three queue managers:

| Queue Manager | Role               |
| ------------- | ------------------ |
| CLQM1         | Full Repository    |
| CLQM2         | Full Repository    |
| CLQM3         | Partial Repository |

**Cluster name:** `MQCLUSTER`

The architecture and queue manager names are examples. You can adapt them to your own lab environment.

## Prerequisites

* IBM MQ installed on Linux systems or virtual machines.
* Basic understanding of queue managers, local queues, and MQ channels.
* Access to configure and administer the queue managers.
* Network connectivity between the systems.
* Basic Linux command-line knowledge.

## Learning Path

1. Prerequisites and Environment
2. MQ Clustering Fundamentals
3. Queue Manager Creation
4. Full and Partial Repositories
5. Cluster Channels
6. Cluster Queues
7. Message Routing
8. Workload Balancing
9. Cluster Troubleshooting
10. Failure Scenarios
11. Security and Best Practices
12. Advanced Capstone

## Lab Environment

The examples are developed and tested incrementally. Commands, configuration examples, and expected outputs will be documented as the labs are completed.

Your environment may differ depending on the IBM MQ version, operating system, network configuration, and deployment architecture.

## Repository Structure

* `labs/` — Step-by-step practical exercises.
* `docs/` — Architecture, concepts, glossary, and reference documentation.
* `diagrams/` — Architecture and message-flow diagrams.
* `scripts/` — Reusable helper scripts.
* `config/` — Example configuration templates.

## Who Is This For?

This repository is intended for IBM MQ beginners, administrators, middleware engineers, and support engineers who want to develop practical knowledge of IBM MQ clustering.

## Contributions

Suggestions, corrections, additional examples, and troubleshooting scenarios are welcome. The goal is to make IBM MQ clustering easier to understand and practice for everyone.
