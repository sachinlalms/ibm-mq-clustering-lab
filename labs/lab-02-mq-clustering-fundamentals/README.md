# Lab 02: IBM MQ Clustering Fundamentals

## Objectives

By the end of this lab, you will be able to:

- Explain what an IBM MQ cluster is.
- Identify the main components of an MQ cluster.
- Explain full repositories and partial repositories.
- Understand cluster sender and cluster receiver channels.
- Explain how cluster queues help route messages.
- Distinguish clustering, workload balancing, and high availability.

## 1. What is an IBM MQ cluster?

An IBM MQ cluster is a group of queue managers that are logically connected and share information about queues and other cluster resources.

Without clustering, administrators commonly configure communication between queue managers using manually maintained remote-queue definitions and sender/receiver channels.

Clustering can reduce this administration. Queue managers exchange cluster information, and MQ can create the required cluster sender channels automatically when connectivity to another cluster queue manager is needed.

A cluster does not mean that all queue managers become one queue manager. Each queue manager remains an independent MQ instance with its own queues, logs, and configuration.

## 2. Why use MQ clustering?

Common reasons include:

- Simplifying communication between multiple queue managers.
- Making queues hosted by different queue managers discoverable within a cluster.
- Routing messages to queues hosted on other queue managers.
- Distributing messages across multiple instances of a cluster queue, subject to MQ routing and workload rules.
- Reducing the number of manually maintained point-to-point definitions.

Clustering does not eliminate the need to plan security, networking, monitoring, capacity, or recovery.

## 3. Example architecture

This learning series uses three example queue managers:

```text
                    MQCLUSTER
         +-----------------------------+
         |                             |
         |  CLQM1                      |
         |  Full Repository            |
         |       |                     |
         |       | Cluster channels    |
         |       |                     |
         |  CLQM2                      |
         |  Full Repository            |
         |       |                     |
         |       |                     |
         |  CLQM3                      |
         |  Partial Repository         |
         |                             |
         +-----------------------------+
```

This is a logical illustration, not a physical network diagram. In the actual lab, each queue manager can run on a separate host.

| Queue manager | Example role |
|---|---|
| CLQM1 | Full repository |
| CLQM2 | Full repository |
| CLQM3 | Partial repository |
| Cluster | MQCLUSTER |

The names are examples and can be changed for your environment.

## 4. What is a repository?

A repository contains information about the queue managers, queues, and other resources in a cluster.

Queue managers use repository information to discover cluster resources and establish communication when required.

### Full repository

A full repository maintains the complete repository information for its cluster.

Key points:

- It participates in exchanging cluster information.
- It helps other queue managers discover cluster resources.
- It must be available and reachable for reliable cluster administration and information exchange.
- It is common practice to configure two full repositories on separate systems to reduce the risk of losing access to repository services.

### Partial repository

A partial repository maintains cluster information relevant to that queue manager and obtains additional information as needed.

Key points:

- It is a member of the cluster.
- It can host cluster queues and applications.
- It communicates with full repositories to obtain cluster information.
- It does not need to hold the complete repository for the entire cluster.

A partial repository is not a lower-capability queue manager. The terms describe how repository information is maintained.

## 5. What are cluster channels?

Channels provide the communication path between queue managers.

### Cluster receiver channel (CLUSRCVR)

A cluster receiver channel defines how another queue manager can connect to this queue manager as part of the cluster.

It includes connection information such as the connection name and listener port.

Conceptually:

```text
Remote queue manager
        |
        | TCP connection
        v
Destination CLUSRCVR
        |
        v
Destination queue manager
```

A cluster receiver channel is configured on the queue manager that receives the connection.

### Cluster sender channel (CLUSSDR)

A cluster sender channel is used to send messages and cluster-related information to another queue manager.

MQ can automatically create cluster sender channels when a route to another cluster queue manager is needed. The exact creation and routing behavior depends on the cluster configuration and available repository information.

### Important distinction

In a basic point-to-point setup, administrators commonly configure sender and receiver channels manually.

In a cluster, administrators define the required cluster configuration and receiver connectivity. MQ can create the necessary cluster sender channels automatically.

Do not assume every cluster channel is always active. Channels start when needed and can stop when idle, depending on configuration and activity.

## 6. What is a cluster queue?

A cluster queue is a queue made available to other queue managers in the cluster.

For example, suppose `CLQM2` hosts a local queue named `APP.ORDERS`. If that queue is defined as a cluster queue, another queue manager in the cluster can discover it and route messages to it without requiring a traditional manually maintained remote-queue definition for every sending queue manager.

The queue still physically belongs to its host queue manager. A cluster queue is not copied to every queue manager.

Example concept:

```text
Application
    |
    v
CLQM1
    |
    | MQ cluster routing
    v
CLQM2
    |
    v
APP.ORDERS (local queue)
```

This illustrates routing to a queue hosted on another queue manager. The actual route depends on the queue definitions, cluster configuration, connectivity, and routing rules.

## 7. How message routing works

At a high level:

1. An application puts a message to a destination queue name.
2. MQ determines whether the destination is local or available through the cluster.
3. MQ uses cluster information to identify an appropriate destination queue manager.
4. If the destination is remote, MQ routes the message through the necessary channel path.
5. The destination queue manager receives the message and places it on the target queue.

Applications must still use the appropriate MQ connection and object names. Cluster membership does not automatically make every application connect to every queue manager.

## 8. Clustering versus workload balancing versus high availability

These concepts are related, but they are not interchangeable.

### Clustering

Clustering allows queue managers to share resource information and route messages between queue managers.

### Workload balancing

If multiple queue managers host instances of the same cluster queue, MQ can distribute messages among eligible destinations according to its routing and workload rules.

Do not assume that every message is sent to a different queue manager or that distribution will always be exactly equal. Configuration, destination availability, message handling, and workload rules affect the result.

### High availability

High availability aims to keep a service available when a component fails. It requires an appropriate availability design for the queue manager, its data, and its infrastructure.

An MQ cluster by itself does not guarantee that a failed queue manager's local queues or unconsumed messages are immediately available on another queue manager.

If an application depends on a particular queue manager or queue, assess the required recovery and availability behavior separately.

## 9. Cluster design considerations

Before implementing a cluster, consider:

- Which queue managers will be full repositories.
- Where the queue managers will run.
- Hostname resolution and TCP connectivity.
- Listener ports and firewall rules.
- Channel authentication, TLS, and MQ authorization.
- Queue names and cluster queue placement.
- Monitoring, backup, recovery, and failure testing.

Do not expose MQ listener ports to untrusted networks. Apply security controls appropriate to the environment.

## 10. Commands for understanding the current environment

The following commands inspect MQ configuration. Run them using the appropriate MQ administrative account and environment.

Display the MQ version:

```bash
dspmqver
```

List queue managers:

```bash
dspmq
```

Display cluster-related channel definitions for a queue manager using MQSC:

```text
DISPLAY CHANNEL(*) CHLTYPE(CLUSSDR)
DISPLAY CHANNEL(*) CHLTYPE(CLUSRCVR)
```

Display cluster queue definitions:

```text
DISPLAY QUEUE(*) TYPE(QCLUSTER)
```

These MQSC commands are examples for later use. They may return no matching definitions if clustering has not yet been configured.

## 11. Verification

You should now be able to explain:

- Why an MQ cluster is used.
- What a full repository does.
- What a partial repository does.
- The difference between CLUSSDR and CLUSRCVR.
- Why a cluster queue remains hosted on a particular queue manager.
- Why clustering alone does not guarantee high availability.

## Troubleshooting

For common conceptual and command-related issues, see [troubleshooting.md](troubleshooting.md).

## Knowledge check

Complete [quiz.md](quiz.md) and review the answers before moving to Lab 03.

## Key takeaways

- Queue managers remain independent members of a cluster.
- Full repositories maintain complete cluster repository information.
- Partial repositories maintain information relevant to their needs.
- Cluster channels provide inter-queue-manager communication.
- Cluster queues allow resources to be discovered and messages to be routed across the cluster.
- Clustering, workload balancing, and high availability solve different problems.
