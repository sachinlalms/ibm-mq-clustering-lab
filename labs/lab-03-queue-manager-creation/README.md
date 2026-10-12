# Lab 03: Queue Manager Creation

## Objectives

By the end of this lab, you will be able to:

- Understand what an IBM MQ queue manager does.
- Check whether a queue manager name is already in use.
- Create a queue manager using `crtmqm`.
- Start and stop a queue manager.
- Verify its status and inspect basic configuration.
- Understand the difference between a queue manager and a queue.

## 1. What is a queue manager?

An IBM MQ queue manager manages messaging resources, including queues, channels, and connections. It accepts MQ API requests from applications and manages messages stored on its queues.

A queue manager has its own configuration, message data, and recovery information. Multiple queue managers can communicate with one another using MQ channels.

In the clustering labs, each queue manager will have a specific role.

| Queue manager | Planned role |
|---|---|
| CLQM1 | Full repository |
| CLQM2 | Full repository |
| CLQM3 | Partial repository |

These are example names. Use names appropriate to your environment.

## 2. Prerequisites

Before proceeding, confirm:

- IBM MQ is installed on each target host.
- You can run MQ administrative commands.
- You have the required permissions to create and start queue managers.
- Sufficient disk space is available.
- The example queue manager names are not already in use.
- You know which host will run each queue manager.

For the later cluster labs, three separate hosts are recommended for a realistic network exercise. A single host can be used for a limited learning exercise, but each queue manager will need distinct connection and listener configuration where applicable.

## 3. Check the MQ environment

Run the following commands on each target host:

```bash
dspmqver
dspmq
```

- `dspmqver` displays IBM MQ version information.
- `dspmq` displays the queue managers known to the installation and their status.

Record the output before making changes.

If the commands are not found, check the MQ installation and environment configuration. Installation paths can differ between systems.

## 4. Understand the creation command

The basic command to create a queue manager is:

```bash
crtmqm CLQM1
```

Replace `CLQM1` with the intended queue manager name.

`crtmqm` creates the queue manager and its required configuration and data directories. The exact directory locations and options depend on the MQ installation and operating system.

Creating a queue manager does not automatically configure it as a cluster member. Cluster configuration is covered in Lab 04.

## 5. Create CLQM1

On the host designated for `CLQM1`, use an MQ administrative account and run:

```bash
crtmqm CLQM1
```

Review the command output. If the command fails, stop and investigate the error before proceeding.

Start the queue manager:

```bash
strmqm CLQM1
```

Check its status:

```bash
dspmq -m CLQM1
```

Expected status after a successful start:

```text
QMNAME(CLQM1) STATUS(Running)
```

Formatting may vary by MQ version and command options.

## 6. Create CLQM2 and CLQM3

Repeat the same process on their designated hosts.

On the host for `CLQM2`:

```bash
crtmqm CLQM2
strmqm CLQM2
dspmq -m CLQM2
```

On the host for `CLQM3`:

```bash
crtmqm CLQM3
strmqm CLQM3
dspmq -m CLQM3
```

Run each creation command only on the host where that queue manager is intended to reside.

If you use one host for multiple queue managers, create each name only once and check the existing queue manager list before proceeding.

Do not rerun `crtmqm` for a queue manager that already exists.

## 7. Inspect basic queue manager configuration

Connect to `CLQM1` using MQSC:

```bash
runmqsc CLQM1
```

At the MQSC prompt, run:

```text
DISPLAY QMGR
```

This displays queue manager attributes, such as its name and other configuration values.

To exit MQSC:

```text
END
```

Repeat these checks for `CLQM2` and `CLQM3` as needed.

## 8. Stop and restart a queue manager

For a controlled learning exercise, a queue manager can be stopped using:

```bash
endmqm CLQM1
```

Check its status:

```bash
dspmq -m CLQM1
```

When the queue manager is stopped and you are ready to restart it:

```bash
strmqm CLQM1
```

The default `endmqm` behavior requests a controlled stop. The command may wait for applications or other activity to finish. Follow the displayed instructions if the stop does not complete.

Use these commands only for the queue manager you intend to control. Do not stop a queue manager that other users or applications depend on.

## 9. Verify all three queue managers

On each host, run:

```bash
dspmq
```

Confirm that the intended queue manager exists and is running.

Complete the following checklist:

- [ ] `CLQM1` created and verified.
- [ ] `CLQM2` created and verified.
- [ ] `CLQM3` created and verified.
- [ ] Each queue manager starts successfully.
- [ ] MQSC can connect to each queue manager.
- [ ] Queue manager names and host assignments recorded.

## 10. Important concepts

### Queue manager versus queue

A queue manager manages queues and other MQ resources. A queue stores messages.

For example, `CLQM1` is a queue manager name. A queue such as `APP.ORDERS` is an MQ object that may be hosted by a queue manager.

### Queue manager versus cluster

Creating queue managers does not create a cluster. In Lab 04, we will configure cluster membership and repository roles.

### Local administration versus remote connectivity

A queue manager can run successfully even if another host cannot connect to it. Cluster communication requires additional configuration, including host connectivity, listeners, and cluster channels.

## Troubleshooting

See [troubleshooting.md](troubleshooting.md).

## Knowledge check

Complete [quiz.md](quiz.md).

## Key takeaways

- `crtmqm` creates a queue manager.
- `strmqm` starts a queue manager.
- `dspmq` displays queue manager status.
- `runmqsc` provides an interface for inspecting and configuring MQ objects.
- Queue manager creation is only the first step; clustering must be configured separately.
