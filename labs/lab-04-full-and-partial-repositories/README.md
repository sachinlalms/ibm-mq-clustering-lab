# Lab 04: Full and Partial Repositories

## Objectives

By the end of this lab, you will be able to:

- Explain full and partial repositories.
- Configure two full repository queue managers.
- Configure a partial repository queue manager.
- Define listeners and cluster channels.
- Verify cluster membership and repository information.
- Troubleshoot common initial cluster connectivity problems.

## 1. Lab architecture

This lab uses three queue managers:

| Queue manager | Repository role | Example cluster |
|---|---|---|
| CLQM1 | Full repository | MQCLUSTER |
| CLQM2 | Full repository | MQCLUSTER |
| CLQM3 | Partial repository | MQCLUSTER |

The intended architecture is:

```text
                   MQCLUSTER
               /               \
       CLQM1 (Full) -------- CLQM2 (Full)
               \               /
                \             /
                 CLQM3 (Partial)
```

The diagram is conceptual. Actual connections are established through cluster channels.

## 2. Prerequisites

Complete Lab 03 first.

Before starting:

- Confirm all three queue managers exist and are running.
- Confirm each queue manager's host can resolve the other hosts.
- Confirm the hosts can communicate over TCP.
- Decide on the hostname or IP address that each queue manager will advertise.
- Confirm that TCP port 1414 is available on each target host, or choose another port and use it consistently.
- Ensure the MQ administrative account has permission to configure each queue manager.

Use hostnames or IP addresses that are reachable from the other MQ hosts. Do not copy example hostnames literally.

**Important:** These instructions assume each queue manager runs on a separate host. If several queue managers share a host, configure distinct listener ports and adjust the connection names accordingly.

## 3. Understand the repository roles

A full repository maintains the complete repository information for the cluster. A partial repository maintains the cluster information relevant to its needs and obtains additional information as required.

For this lab:

- CLQM1 is the first full repository.
- CLQM2 is the second full repository.
- CLQM3 is a partial repository.

Two full repositories on separate systems are a common design for resilience of repository services. They should be configured to communicate with each other.

## 4. Record the host information

On each host, run:

```bash
hostname -f
dspmq
dspmqver
```

Record the actual hostname or IP address used by each queue manager:

| Queue manager | Advertised host | Listener port |
|---|---|---|
| CLQM1 | <CLQM1_HOST> | 1414 |
| CLQM2 | <CLQM2_HOST> | 1414 |
| CLQM3 | <CLQM3_HOST> | 1414 |

Replace the angle-bracket placeholders in the MQSC commands below with the real hostnames or IP addresses. The angle brackets are explanatory placeholders, not literal MQSC syntax.

## 5. Configure CLQM1 as a full repository

Connect to CLQM1:

```bash
runmqsc CLQM1
```

### Step 5.1: Set the full repository role

```text
ALTER QMGR REPOS(MQCLUSTER)
```

This makes CLQM1 a full repository for MQCLUSTER.

### Step 5.2: Define a listener

If no suitable listener already exists, define one:

```text
DEFINE LISTENER(CLQM1.LISTENER) TRPTYPE(TCP) PORT(1414) CONTROL(QMGR)
START LISTENER(CLQM1.LISTENER)
```

If an existing listener already provides the required port and behavior, verify and reuse it rather than creating a conflicting listener.

### Step 5.3: Define the cluster receiver

Replace <CLQM1_HOST> with CLQM1's reachable hostname or IP address:

```text
DEFINE CHANNEL(MQCLUSTER.CLQM1) CHLTYPE(CLUSRCVR) TRPTYPE(TCP) CONNAME('<CLQM1_HOST>(1414)') CLUSTER(MQCLUSTER)
```

The receiver advertises how other queue managers can connect to CLQM1.

### Step 5.4: Define the sender to the second full repository

CLQM1 must have a manually defined cluster sender to CLQM2's receiver:

```text
DEFINE CHANNEL(MQCLUSTER.CLQM2) CHLTYPE(CLUSSDR) TRPTYPE(TCP) CONNAME('<CLQM2_HOST>(1414)') CLUSTER(MQCLUSTER)
```

Replace <CLQM2_HOST> with CLQM2's reachable hostname or IP address.

Exit MQSC:

```text
END
```

## 6. Configure CLQM2 as a full repository

Connect to CLQM2:

```bash
runmqsc CLQM2
```

Set the full repository role:

```text
ALTER QMGR REPOS(MQCLUSTER)
```

Define and start the listener if an equivalent listener does not already exist:

```text
DEFINE LISTENER(CLQM2.LISTENER) TRPTYPE(TCP) PORT(1414) CONTROL(QMGR)
START LISTENER(CLQM2.LISTENER)
```

Define the cluster receiver. Replace <CLQM2_HOST> with CLQM2's reachable address:

```text
DEFINE CHANNEL(MQCLUSTER.CLQM2) CHLTYPE(CLUSRCVR) TRPTYPE(TCP) CONNAME('<CLQM2_HOST>(1414)') CLUSTER(MQCLUSTER)
```

Define the sender to CLQM1. Replace <CLQM1_HOST> with CLQM1's reachable address:

```text
DEFINE CHANNEL(MQCLUSTER.CLQM1) CHLTYPE(CLUSSDR) TRPTYPE(TCP) CONNAME('<CLQM1_HOST>(1414)') CLUSTER(MQCLUSTER)
```

Exit MQSC:

```text
END
```

The sender and receiver channel names match at each destination. The two full repositories can now establish their initial connection and exchange cluster repository information.

## 7. Configure CLQM3 as a partial repository

Connect to CLQM3:

```bash
runmqsc CLQM3
```

A newly created queue manager is normally a partial repository unless explicitly configured as a full repository. For this lab, do not set REPOS on CLQM3.

Define and start a listener if an equivalent listener does not already exist:

```text
DEFINE LISTENER(CLQM3.LISTENER) TRPTYPE(TCP) PORT(1414) CONTROL(QMGR)
START LISTENER(CLQM3.LISTENER)
```

Define its cluster receiver. Replace <CLQM3_HOST> with CLQM3's reachable address:

```text
DEFINE CHANNEL(MQCLUSTER.CLQM3) CHLTYPE(CLUSRCVR) TRPTYPE(TCP) CONNAME('<CLQM3_HOST>(1414)') CLUSTER(MQCLUSTER)
```

Define one initial sender to a full repository, CLQM1:

```text
DEFINE CHANNEL(MQCLUSTER.CLQM1) CHLTYPE(CLUSSDR) TRPTYPE(TCP) CONNAME('<CLQM1_HOST>(1414)') CLUSTER(MQCLUSTER)
```

Exit MQSC:

```text
END
```

The initial sender gives CLQM3 a way to contact the cluster. Once connected and informed by the repositories, MQ can create additional cluster sender channels automatically when needed.

## 8. Verify the configuration

Run the following on each queue manager using MQSC.

Check the queue manager name and repository setting:

```text
DISPLAY QMGR QMNAME REPOS
```

Expected repository roles:

- CLQM1: REPOS(MQCLUSTER)
- CLQM2: REPOS(MQCLUSTER)
- CLQM3: REPOS(blank)

The exact display formatting can vary by MQ version.

Check the receiver definitions:

```text
DISPLAY CHANNEL(*) CHLTYPE(CLUSRCVR)
```

Check manually configured sender definitions:

```text
DISPLAY CHANNEL(*) CHLTYPE(CLUSSDR)
```

Check the cluster queue manager information:

```text
DISPLAY CLUSQMGR(*)
```

Allow time for channels to connect and repository information to propagate. Some auto-defined cluster sender channels may not appear until they are needed.

## 9. Verify channel status

Use MQSC on the relevant queue manager:

```text
DISPLAY CHSTATUS(*)
```

Look for cluster channels that are active or have successfully connected. The channel status display may be empty when no channel is active, so also inspect the channel definitions and error logs.

If a channel is not connecting, verify:

- The destination hostname resolves correctly.
- The destination listener is running.
- The configured port matches the listener.
- Firewalls allow the connection.
- The sender channel name matches the receiver channel name at the destination.
- The cluster name and channel definitions are correct.

## 10. Important operational notes

- Do not manually define cluster sender channels to partial repositories. MQ creates them automatically when required.
- Manually define cluster sender channels between full repositories and the initial sender channel from a partial repository to a full repository.
- Keep matching channel properties consistent where applicable.
- A successful local queue manager start does not prove cluster connectivity.
- Avoid using REFRESH CLUSTER as a routine troubleshooting step. It can affect cluster operation and should only be used when justified by the documented procedure.
- These lab commands do not configure channel authentication or TLS. Add appropriate security before using a cluster beyond an isolated learning environment.

## Troubleshooting

See [troubleshooting.md](troubleshooting.md).

## Knowledge check

Complete [quiz.md](quiz.md).

## Key takeaways

- Full repositories maintain complete cluster repository information.
- Partial repositories obtain and maintain the cluster information they need.
- Each queue manager needs a cluster receiver channel.
- Full repositories have manually defined sender channels to the other full repositories.
- A partial repository needs one initial sender channel to a full repository.
- MQ can define additional cluster sender channels automatically when required.

## Official IBM documentation

- [Setting up a cluster using TCP/IP](https://www.ibm.com/docs/en/ibm-mq/9.4.x?topic=sunc-setting-up-cluster-using-tcpip-single-transmission-queue-per-queue-manager)
- [Adding a queue manager to a cluster](https://www.ibm.com/docs/en/ibm-mq/9.4.x?topic=cluster-adding-queue-manager)
- [Cluster channels](https://www.ibm.com/docs/en/ibm-mq/9.3.x?topic=components-cluster-channels)
