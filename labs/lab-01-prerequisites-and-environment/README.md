# Lab 01: Prerequisites and Environment

## Objectives

By the end of this lab, you will be able to:

- Identify the basic components required for IBM MQ clustering.
- Verify the IBM MQ installation and version.
- Check hostnames, name resolution, and network connectivity.
- Prepare an environment for the clustering labs.

## Prerequisites

You need:

- Access to Linux systems where IBM MQ is installed.
- Permission to run IBM MQ administrative commands.
- Basic Linux command-line knowledge.
- Network connectivity between the systems used for the lab.
- Hostnames or IP addresses for the systems in your environment.

For the complete clustering exercise, prepare three queue managers on systems that can communicate with one another. The queue managers may run on separate hosts or on one host for limited learning exercises, provided the required ports and names are configured correctly.

## Theory

An IBM MQ cluster is a group of queue managers that can share information about queues and route messages between queue managers.

The lab will use these example names:

| Component | Example |
|---|---|
| Cluster | MQCLUSTER |
| Queue manager 1 | CLQM1 |
| Queue manager 2 | CLQM2 |
| Queue manager 3 | CLQM3 |
| Example listener port | 1414 |

These are example values. Replace them with values suitable for your environment.

A typical learning design uses two full repository queue managers and one partial repository queue manager. The full repositories maintain the cluster's shared information. A partial repository obtains cluster information as required.

## Step 1: Check the operating system

Run these commands on each Linux host:

```bash
hostname
hostname -f
cat /etc/os-release
uname -m
```

Record the hostname, operating system, and architecture for each host.

## Step 2: Verify IBM MQ installation

Run the following as a user with the required IBM MQ environment and permissions:

```bash
dspmqver
dspmq
```

`dspmqver` displays IBM MQ version information. `dspmq` lists queue managers known to the installation and their status.

If a command is not found, check whether IBM MQ is installed and whether the MQ environment has been configured. Do not assume that every installation uses the same installation path.

## Step 3: Check hostname resolution

For each host, check that its hostname resolves to the intended address:

```bash
getent hosts <hostname>
```

Replace `<hostname>` with the actual hostname of a lab system.

Confirm that each participating host can resolve the other hosts using the names that will be configured in MQ channel definitions.

## Step 4: Check network connectivity

From each participating host, test connectivity to the intended destination host:

```bash
ping -c 4 <destination-host>
```

If ICMP traffic is blocked, a failed ping does not necessarily mean MQ connectivity is unavailable.

When the destination listener is configured and running, test its TCP port:

```bash
nc -vz <destination-host> 1414
```

Replace the host and port with the values used in your environment. A connection test will fail if the listener is not running or the network path is blocked, so perform this check after the listener has been configured.

## Step 5: Record the lab design

Before creating queue managers, record the intended configuration:

| Setting | Value |
|---|---|
| Cluster name | MQCLUSTER |
| Queue manager 1 | CLQM1 |
| Queue manager 1 host | <host-1> |
| Queue manager 2 | CLQM2 |
| Queue manager 2 host | <host-2> |
| Queue manager 3 | CLQM3 |
| Queue manager 3 host | <host-3> |
| MQ listener port | 1414 |

Keep a record of which queue manager will act as each full repository and which will act as the partial repository.

## Verification checklist

- [ ] Linux hostnames and operating system versions recorded.
- [ ] IBM MQ version verified.
- [ ] Existing queue managers and their statuses reviewed.
- [ ] Hostname resolution checked between participating hosts.
- [ ] Required network paths and firewall rules reviewed.
- [ ] Cluster and queue manager names selected.
- [ ] Lab configuration recorded.

## Troubleshooting

See [troubleshooting.md](troubleshooting.md).

## Knowledge check

Complete the questions in [quiz.md](quiz.md) before moving to Lab 02.

## Key takeaways

- Verify the environment before changing MQ configuration.
- Hostname resolution and TCP connectivity are important for cluster channels.
- Use consistent names and document the design before building the cluster.
- The commands and example values in this repository must be adapted to the target environment.
