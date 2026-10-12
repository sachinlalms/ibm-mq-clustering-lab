# Lab 03: Queue Manager Creation Troubleshooting

## 1. `crtmqm` reports that the queue manager already exists

Possible causes:

- The queue manager was created previously.
- The name is already registered in the MQ installation.
- You are using a name that is already allocated in the current environment.

Diagnostic command:

```bash
dspmq
```

Resolution:

Check whether the queue manager exists and whether it is the intended one. Do not delete or recreate an existing queue manager simply to clear the error. Choose another name if necessary.

## 2. `crtmqm` fails with a permission error

Possible causes:

- The command was run as an account without the required MQ administrative permissions.
- The installation environment is not configured correctly.
- Required directories or files are not accessible.

Resolution:

Review the complete error message. Confirm the account, MQ installation, and directory permissions according to your organization's MQ administration procedures.

Do not apply broad permission changes to MQ directories as a workaround.

## 3. `crtmqm` fails because of insufficient disk space

Check filesystem capacity:

```bash
df -h
```

Check available inodes:

```bash
df -i
```

Resolution:

Confirm the filesystems used by the MQ installation and queue manager data and log paths have sufficient capacity. Follow your site's storage requirements before retrying.

## 4. `strmqm` fails to start the queue manager

Check the current status:

```bash
dspmq -m CLQM1
```

Review the MQ error logs and the complete output from `strmqm`.

Possible causes include:

- Filesystem or permission issues.
- Incorrect or unavailable storage paths.
- Resource limitations.
- Configuration problems.
- Another process or service interfering with startup.

Resolution:

Investigate the specific error before attempting another start. Replace `CLQM1` with the queue manager being investigated.

## 5. `runmqsc CLQM1` cannot connect

Confirm the queue manager name and status:

```bash
dspmq -m CLQM1
```

Possible causes:

- The queue manager is stopped.
- The name is incorrect.
- The MQ environment is not configured.
- The command is being run against an installation that does not know the queue manager.

Resolution:

Verify the local installation and queue manager status. For local MQSC administration, a network listener is not generally required.

## 6. The queue manager remains in a stopping state

A controlled stop may take time while applications or MQ activity finish.

Resolution:

- Check the output from `endmqm`.
- Check whether applications are still connected.
- Review the MQ error logs.
- Follow the site's operational procedure before attempting any forced stop.

Do not use an immediate or forced stop as the first troubleshooting step.

## Diagnostic information

Capture these commands when investigating:

```bash
dspmqver
dspmq
df -h
df -i
```

Also record the exact command used, its full output, the queue manager name, and relevant MQ error-log entries.
