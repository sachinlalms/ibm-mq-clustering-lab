# Lab 02: Knowledge Check

## Questions

1. What is an IBM MQ cluster?
2. Does clustering combine several queue managers into one queue manager?
3. What information does a full repository maintain?
4. Does a partial repository maintain the complete cluster repository?
5. What is the purpose of a CLUSRCVR channel?
6. What is the purpose of a CLUSSDR channel?
7. What is a cluster queue?
8. Does a cluster queue get copied to every queue manager in the cluster?
9. Does MQ clustering guarantee perfectly equal workload distribution?
10. Does MQ clustering alone guarantee that a failed queue manager's local messages are available on another queue manager?
11. Why are hostname resolution and listener connectivity important?
12. Which command displays cluster queue definitions?

## Answers

1. It is a group of queue managers that share cluster information and can route messages between queue managers.
2. No. Each queue manager remains an independent MQ instance.
3. A full repository maintains the complete repository information for the cluster.
4. No. A partial repository maintains the information relevant to its needs and obtains additional information as required.
5. It defines how other queue managers can connect to the receiving queue manager as part of the cluster.
6. It is used to send messages and cluster-related information to another queue manager.
7. It is a queue made available to other queue managers through the cluster.
8. No. The queue remains hosted on the queue manager where it is defined.
9. No. Distribution depends on MQ routing and workload rules, destination eligibility, availability, and configuration.
10. No. Queue manager and data availability require a separate appropriate high-availability and recovery design.
11. Cluster channels require working network connectivity to the intended destination host and listener.
12. `DISPLAY QUEUE(*) TYPE(QCLUSTER)` in MQSC.
