# Lab 03: Queue Manager Creation Knowledge Check

## Questions

1. What is the purpose of an IBM MQ queue manager?
2. Which command creates a queue manager?
3. Which command starts a queue manager?
4. Which command displays queue manager status?
5. Which command opens an MQSC session for a local queue manager?
6. Does creating a queue manager automatically make it a member of an MQ cluster?
7. What is the difference between a queue manager and a queue?
8. What should you do if `crtmqm` reports that the queue manager already exists?
9. Is a listener always required to run local MQSC commands against a running queue manager?
10. Why should you investigate a failed queue manager start before trying repeated restarts?
11. What does `endmqm CLQM1` request?
12. Why should queue manager names and host assignments be documented?

## Answers

1. It manages MQ resources, including queues, channels, and message processing.
2. `crtmqm <QMNAME>`, for example `crtmqm CLQM1`.
3. `strmqm <QMNAME>`, for example `strmqm CLQM1`.
4. `dspmq`, or `dspmq -m <QMNAME>` for a particular queue manager.
5. `runmqsc <QMNAME>`.
6. No. Cluster membership requires separate configuration.
7. A queue manager manages MQ resources; a queue stores messages.
8. Check `dspmq`, verify the existing queue manager, and choose another name if appropriate. Do not delete an existing queue manager without a valid, approved reason.
9. No. Local MQSC administration generally does not require a network listener.
10. Repeated attempts may not resolve the underlying issue and can complicate diagnosis.
11. It requests a controlled stop of the specified queue manager.
12. Documentation helps avoid naming conflicts and makes subsequent cluster configuration and troubleshooting easier.
