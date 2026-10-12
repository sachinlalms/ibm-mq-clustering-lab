# Lab 04: Knowledge Check

## Questions

1. What is the purpose of a full repository?
2. What is the purpose of a partial repository?
3. Which MQSC attribute assigns a queue manager as a full repository for a cluster?
4. Should CLQM3 be configured with `REPOS(MQCLUSTER)` in this lab?
5. Why does each queue manager need a cluster receiver channel?
6. How many initial cluster sender channels should a partial repository define for joining the cluster?
7. Between which queue managers should sender channels be manually defined for repository communication?
8. Why should the sender channel name match the destination's cluster receiver channel name?
9. Which MQSC command displays the repository role?
10. Which command displays known cluster queue managers?
11. Does a channel definition guarantee that the channel is currently active?
12. Why should `REFRESH CLUSTER` not be used routinely as the first troubleshooting step?

## Answers

1. It maintains the complete repository information for the cluster.
2. It maintains relevant cluster information and obtains additional information as needed.
3. `ALTER QMGR REPOS(MQCLUSTER)`.
4. No. CLQM3 is intended to be a partial repository and should not have the cluster name configured in its REPOS attribute.
5. The receiver advertises how the queue manager can receive cluster connections and enables other queue managers to connect.
6. One initial sender channel to a full repository.
7. Between each full repository and the other full repository or repositories. In this lab, CLQM1 and CLQM2 each have a sender channel to the other.
8. The sender must connect to the corresponding receiver definition at the destination.
9. `DISPLAY QMGR QMNAME REPOS`.
10. `DISPLAY CLUSQMGR(*)`.
11. No. A channel definition can exist without an active channel instance.
12. It can affect cluster operation. Connectivity, definitions, and logs should be investigated first.
