# Lab 02: Troubleshooting

This lab introduces concepts and inspection commands. Most configuration troubleshooting is covered in later labs.

## 1. No cluster sender channels are displayed

Command:

```text
DISPLAY CHANNEL(*) CHLTYPE(CLUSSDR)
```

Possible explanations:

- The queue manager is not configured as a cluster member.
- Cluster sender channels have not yet been created.
- The current configuration does not require a sender channel to the expected destination.
- The command was run against a different queue manager than intended.

Next steps:

- Confirm the queue manager name.
- Review its cluster-related configuration.
- Check the relevant cluster receiver definitions and repository configuration.

Do not conclude that the entire cluster is broken solely because no cluster sender channel is displayed.

## 2. No cluster receiver channels are displayed

Command:

```text
DISPLAY CHANNEL(*) CHLTYPE(CLUSRCVR)
```

Possible explanations:

- A cluster receiver channel has not been defined.
- The command was run against the wrong queue manager.
- The queue manager has not yet been configured for cluster membership.

Next steps:

- Confirm the intended queue manager.
- Review its channel definitions.
- Verify the connection name and listener port when the channel is configured.

## 3. No cluster queues are displayed

Command:

```text
DISPLAY QUEUE(*) TYPE(QCLUSTER)
```

Possible explanations:

- No local queues have been defined as cluster queues.
- The queue definition has not been configured with the intended cluster name.
- The command was run against a queue manager that does not host the expected cluster queue.

Next steps:

- Check the local queue definition on the queue manager hosting the queue.
- Confirm the cluster name and queue definition.
- Verify that the queue manager is correctly configured as a cluster member.

## 4. A queue manager cannot connect to another queue manager

Check the following:

- Hostname resolution.
- Listener status and port.
- Firewall and network routing.
- Cluster receiver channel connection information.
- Channel authentication and TLS configuration.
- MQ error logs and channel status.

Useful operating-system checks include:

```bash
getent hosts <destination-host>
nc -vz <destination-host> 1414
```

Replace the example hostname and port with the values used in your environment. A TCP test requires a listening service at the destination.

## 5. Messages are not reaching the expected queue

Check the following:

- The target queue exists and is available.
- The target queue is configured as intended for cluster use.
- The queue manager has the required cluster information.
- The relevant cluster channels can connect.
- The destination is eligible for routing.
- MQ error logs show no authentication, authorization, or connectivity errors.

Use the detailed diagnostic steps in the later cluster queues and message routing labs.

## Diagnostic commands

Run MQSC commands against the intended queue manager:

```text
DISPLAY QMGR
DISPLAY CHANNEL(*) CHLTYPE(CLUSSDR)
DISPLAY CHANNEL(*) CHLTYPE(CLUSRCVR)
DISPLAY QUEUE(*) TYPE(QCLUSTER)
DISPLAY CLUSQMGR(*)
```

Some commands may return no matching objects if the relevant configuration has not been created.

## Important note

Use the exact MQ error messages and queue manager logs when investigating a real failure. Avoid changing channel or repository definitions until the cause is understood.
