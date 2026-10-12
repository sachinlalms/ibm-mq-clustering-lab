# Lab 04: Full and Partial Repository Troubleshooting

## 1. Repository role is incorrect

Run MQSC:

```text
DISPLAY QMGR QMNAME REPOS
```

Expected:

- CLQM1: REPOS(MQCLUSTER)
- CLQM2: REPOS(MQCLUSTER)
- CLQM3: REPOS(blank)

If a queue manager has the wrong role, review the intended design and the current configuration before changing it.

For a queue manager that is intended to be a full repository:

```text
ALTER QMGR REPOS(MQCLUSTER)
```

Do not change repository roles casually on an established cluster. Repository-role changes require planning and the appropriate IBM MQ procedure.

## 2. Cluster sender channel is not connecting

Inspect the channel definition:

```text
DISPLAY CHANNEL(MQCLUSTER.CLQM1) ALL
```

Use the actual sender channel name on the queue manager being investigated.

Check the channel status:

```text
DISPLAY CHSTATUS(*)
```

Possible causes:

- Incorrect destination hostname or IP address.
- Incorrect destination port.
- Listener not running.
- Firewall or network path blocking TCP.
- Sender channel name does not match the receiver channel name at the destination.
- Channel authentication, TLS, or other security settings prevent connection.

Test host resolution and TCP connectivity from the sending host:

```bash
getent hosts <destination-host>
nc -vz <destination-host> 1414
```

Replace the placeholder and port with the actual destination values.

## 3. Listener cannot start

Check the listener definition:

```text
DISPLAY LISTENER(*) ALL
```

Check whether the port is already in use on the host:

```bash
ss -lnt
```

Possible causes:

- Another service is using the port.
- A listener with conflicting settings already exists.
- The listener definition has an invalid port or transport configuration.

Resolution:

Use an available port and ensure that the chosen port is consistent with the cluster receiver connection name and network rules.

## 4. Cluster queue manager information is missing

Run:

```text
DISPLAY CLUSQMGR(*)
```

Possible causes:

- The queue manager has not established a connection to a full repository.
- The sender or receiver channel definitions are incorrect.
- Repository information has not yet propagated.
- The cluster name is inconsistent.
- Network or security errors prevent communication.

Allow time for cluster information to propagate after connectivity is restored.

## 5. Channel definitions exist, but no active channel status appears

Run:

```text
DISPLAY CHANNEL(*) CHLTYPE(CLUSSDR)
DISPLAY CHANNEL(*) CHLTYPE(CLUSRCVR)
DISPLAY CHSTATUS(*)
```

A channel definition can exist without a currently active channel instance. Some cluster sender channels are created automatically only when required.

Check the relevant channel definitions, destination listener, network path, and MQ error logs before assuming the cluster is broken.

## 6. MQSC returns an object-already-exists error

Possible causes:

- The channel or listener was created in an earlier attempt.
- A definition already exists with the same name.

Resolution:

Inspect the existing object:

```text
DISPLAY CHANNEL(<channel-name>) ALL
DISPLAY LISTENER(<listener-name>) ALL
```

Use the appropriate object name. Verify that the existing definition is correct before altering or replacing it. Do not delete definitions blindly.

## 7. Where to look for errors

Review the queue manager error logs under the applicable MQ installation and queue manager data paths. On Linux, these commonly include the queue manager's `errors` directory.

Useful information to collect:

- Queue manager name.
- Channel name and type.
- Full channel status and error details.
- Destination hostname and port.
- Listener status.
- Relevant MQ error-log entries.
- Results of hostname resolution and TCP connectivity tests.

Avoid repeated `REFRESH CLUSTER` commands as a first response. Diagnose the channel and repository connectivity first.
