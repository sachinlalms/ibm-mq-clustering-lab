# Lab 01: Knowledge Check

## Questions

1. What does `dspmqver` display?
2. What does `dspmq` display?
3. Why must participating hosts resolve the hostnames used in MQ channel definitions?
4. Does a failed ping always mean MQ TCP connectivity is unavailable?
5. What does `nc -vz <host> 1414` test?
6. What is the difference between a full repository and a partial repository in an MQ cluster?
7. Why should you record hostnames, queue manager names, and ports before configuring the cluster?

## Answers

1. It displays IBM MQ version and installation information.
2. It lists known queue managers and their current status.
3. MQ channel connections require the configured destination names to resolve to the correct network addresses.
4. No. ICMP may be blocked even when the required TCP connection works.
5. It tests whether a TCP connection can be established to the specified host and port.
6. A full repository maintains the cluster's complete repository information. A partial repository maintains cluster information relevant to its needs and obtains additional information when required.
7. Recording the design reduces configuration mistakes and makes troubleshooting easier.
