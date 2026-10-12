# IBM MQ Clustering Lab

A beginner-friendly, hands-on learning repository for IBM MQ clustering.

## Goals

- Understand IBM MQ cluster architecture and terminology.
- Build a cluster with two Full Repository queue managers and one Partial Repository queue manager.
- Configure cluster channels and cluster queues.
- Observe message routing and workload balancing.
- Troubleshoot common clustering issues.
- Practice safe operational procedures.

## Planned Lab Environment

| Node | Queue Manager | Role |
|---|---|---|
| Node 1 | CLQM1 | Full Repository |
| Node 2 | CLQM2 | Full Repository |
| Node 3 | CLQM3 | Partial Repository |

Cluster name: `MQCLUSTER`

The example names are configurable. Verify your existing queue managers, network,
ports, firewall rules, and system resources before running commands.

## Learning Path

1. [Prerequisites and Environment](labs/lab-01-prerequisites-and-environment/README.md)
2. [MQ Clustering Fundamentals](labs/lab-02-mq-clustering-fundamentals/README.md)
3. [Queue Manager Creation](labs/lab-03-queue-manager-creation/README.md)
4. [Full and Partial Repositories](labs/lab-04-full-and-partial-repositories/README.md)
5. [Cluster Channels](labs/lab-05-cluster-channels/README.md)
6. [Cluster Queues](labs/lab-06-cluster-queues/README.md)
7. [Message Routing](labs/lab-07-message-routing/README.md)
8. [Workload Balancing](labs/lab-08-workload-balancing/README.md)
9. [Cluster Troubleshooting](labs/lab-09-cluster-troubleshooting/README.md)
10. [Failure Scenarios](labs/lab-10-failure-scenarios/README.md)
11. [Security and Best Practices](labs/lab-11-security-and-best-practices/README.md)
12. [Advanced Capstone](labs/lab-12-advanced-capstone/README.md)

## Safety

- Do not modify existing production or RDQM queue managers for these exercises.
- Verify queue manager names, ports, and data paths before executing commands.
- Never commit passwords, certificates with private keys, customer data, or sensitive diagnostic output.
- Perform failure testing only in an isolated lab.

## Compatibility

The initial lab is being developed using IBM MQ 9.4.0.20 on RHEL 9.8.
Some commands and configuration details may vary by environment.

## Contributions

Contributions that improve explanations, reproducibility, and troubleshooting
examples are welcome.
