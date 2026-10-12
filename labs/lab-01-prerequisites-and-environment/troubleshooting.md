# Lab 01: Troubleshooting

## Symptom: `dspmqver` or `dspmq` is not found

Possible causes:

- IBM MQ is not installed.
- The MQ installation environment has not been configured.
- The command is not available in the current user's PATH.

Diagnostic commands:

```bash
echo "$PATH"
command -v dspmqver
command -v dspmq
```

Resolution:

Confirm the installation and configure the correct MQ environment according to your installation's documentation.

## Symptom: A hostname does not resolve

Diagnostic command:

```bash
getent hosts <hostname>
```

Possible causes:

- Incorrect hostname.
- Missing or incorrect DNS record.
- Incorrect local hosts-file entry.
- DNS or network configuration problem.

Resolution:

Verify the hostname and its intended address with the system or network administrator.

## Symptom: TCP connectivity to the MQ port fails

Diagnostic command:

```bash
nc -vz <destination-host> 1414
```

Possible causes:

- The destination listener is not running.
- The wrong hostname or port is being used.
- A firewall blocks the connection.
- A routing or network issue prevents connectivity.

Resolution:

Confirm the listener configuration and status, then check the network path and firewall rules. Test again after the listener is running.

## Symptom: `ping` fails

A failed ping alone does not prove that MQ connectivity is unavailable. ICMP traffic may be blocked.

Resolution:

Check hostname resolution and test the required TCP port directly once the destination listener is available.

## Diagnostic information to record

Capture the following for each host:

```bash
hostname -f
cat /etc/os-release
uname -m
dspmqver
dspmq
```

Record the failed hostname or port test, the source and destination hosts, and the exact error message.
