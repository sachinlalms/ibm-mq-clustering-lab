# IBM MQ Clustering Lab

A beginner-friendly, hands-on learning repository for understanding IBM MQ clustering through practical exercises, configuration examples, architecture diagrams, and troubleshooting scenarios.

This repository is designed to help beginners build a strong foundation in IBM MQ clustering, from the basic concepts to advanced configuration and troubleshooting.

## Learning Objectives

By completing these labs, you will learn how to:

* Understand IBM MQ clustering architecture and terminology.
* Explain the differences between Full Repository and Partial Repository queue managers.
* Configure cluster sender and cluster receiver channels.
* Create and advertise cluster queues.
* Understand how queue managers discover cluster resources.
* Follow message routing between queue managers.
* Understand workload balancing and destination selection.
* Troubleshoot common cluster configuration and connectivity issues.
* Practice failure scenarios and understand their impact on message routing.
* Apply security and operational best practices.

## Learning Path

Follow the labs in sequence for the best learning experience.

| Lab | Topic                                                                                | Description                                                                             |
| --- | ------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------- |
| 01  | [Prerequisites and Environment](labs/lab-01-prerequisites-and-environment/README.md) | Prepare the environment and verify IBM MQ installation, networking, and connectivity.   |
| 02  | [MQ Clustering Fundamentals](labs/lab-02-mq-clustering-fundamentals/README.md)       | Learn clustering concepts, architecture, benefits, and terminology.                     |
| 03  | [Queue Manager Creation](labs/lab-03-queue-manager-creation/README.md)               | Create and verify the queue managers used in the lab.                                   |
| 04  | [Full and Partial Repositories](labs/lab-04-full-and-partial-repositories/README.md) | Understand repository roles and how cluster information is maintained.                  |
| 05  | [Cluster Channels](labs/lab-05-cluster-channels/README.md)                           | Configure and understand cluster sender and receiver channels.                          |
| 06  | [Cluster Queues](labs/lab-06-cluster-queues/README.md)                               | Create cluster queues and understand how they are advertised.                           |
| 07  | [Message Routing](labs/lab-07-message-routing/README.md)                             | Trace messages between queue managers through the cluster.                              |
| 08  | [Workload Balancing](labs/lab-08-workload-balancing/README.md)                       | Explore destination selection when multiple queue managers host the same cluster queue. |
| 09  | [Cluster Troubleshooting](labs/lab-09-cluster-troubleshooting/README.md)             | Diagnose channel failures, connectivity problems, and repository issues.                |
| 10  | [Failure Scenarios](labs/lab-10-failure-scenarios/README.md)                         | Observe the effects of unavailable destinations and failed cluster connections.         |
| 11  | [Security and Best Practices](labs/lab-11-security-and-best-practices/README.md)     | Learn about channel security, TLS, access controls, and operational practices.          |
| 12  | [Advanced Capstone](labs/lab-12-advanced-capstone/README.md)                         | Apply the concepts by building and troubleshooting a complete cluster.                  |

## Lab Architecture

The initial learning scenario uses three queue managers:

| Queue Manager | Role               |
| ------------- | ------------------ |
| `CLQM1`       | Full Repository    |
| `CLQM2`       | Full Repository    |
| `CLQM3`       | Partial Repository |

**Cluster name:** `MQCLUSTER`

The names above are examples and can be changed to suit your environment.

The lab will progressively introduce cluster channels, cluster queues, repository discovery, message routing, workload balancing, and troubleshooting.

## Prerequisites

Before starting the hands-on exercises, you should have:

* Access to an environment with IBM MQ installed.
* Basic knowledge of Linux command-line operations.
* A basic understanding of queue managers, queues, and MQ channels.
* Permission to administer the queue managers used in the exercises.
* Network connectivity between the systems participating in the cluster.
* Familiarity with basic MQ commands such as `dspmq`, `runmqsc`, and `strmqm`.

Some advanced labs may require additional configuration or tools. These requirements will be documented in the relevant lab.

## Repository Structure

```text
ibm-mq-clustering-lab/
├── README.md
├── .gitignore
├── docs/
│   ├── architecture.md
│   ├── prerequisites.md
│   ├── mq-clustering-glossary.md
│   ├── troubleshooting-guide.md
│   └── production-best-practices.md
├── diagrams/
├── config/
│   └── lab.env.example
├── scripts/
└── labs/
    ├── lab-01-prerequisites-and-environment/
    ├── lab-02-mq-clustering-fundamentals/
    ├── lab-03-queue-manager-creation/
    ├── lab-04-full-and-partial-repositories/
    ├── lab-05-cluster-channels/
    ├── lab-06-cluster-queues/
    ├── lab-07-message-routing/
    ├── lab-08-workload-balancing/
    ├── lab-09-cluster-troubleshooting/
    ├── lab-10-failure-scenarios/
    ├── lab-11-security-and-best-practices/
    └── lab-12-advanced-capstone/
```

Each lab is intended to contain:

* `README.md` — Concepts and step-by-step instructions.
* `mqsc/` — MQSC configuration examples.
* `scripts/` — Helper scripts where applicable.
* `expected-output/` — Sample command output and verification results.
* `diagrams/` — Lab-specific architecture or message-flow diagrams.
* `troubleshooting.md` — Common problems and diagnostic steps.
* `quiz.md` — Questions to reinforce learning.

## How to Use This Repository

1. Clone the repository:

   ```bash
   git clone https://github.com/sachinlalms/ibm-mq-clustering-lab.git
   ```

2. Enter the repository directory:

   ```bash
   cd ibm-mq-clustering-lab
   ```

3. Open the first lab:

   ```text
   labs/lab-01-prerequisites-and-environment/README.md
   ```

4. Follow each lab in sequence, executing the commands and reviewing the expected results.

5. Use the troubleshooting guides and quizzes to reinforce your understanding.

## Documentation Approach

The labs aim to explain not only **which commands to run**, but also:

* What each command does.
* Why a configuration is required.
* How the components communicate.
* What output to expect.
* How to verify that the configuration works.
* How to investigate common failures.

Examples and expected outputs will be added and refined as the labs are implemented and tested.

## Compatibility

The examples are developed incrementally and will document the IBM MQ version and operating system used for testing.

Configuration details, available features, and command behavior may vary between IBM MQ versions and environments. Refer to the documentation for your installed IBM MQ version when adapting the examples.

## Who Is This For?

This repository is intended for:

* IBM MQ beginners.
* Middleware and messaging administrators.
* Technical support engineers.
* Infrastructure and platform engineers.
* Anyone who wants practical experience with IBM MQ clustering.

## Contributing

Contributions are welcome!

You can help improve this repository by:

* Correcting errors or clarifying explanations.
* Adding tested configuration examples.
* Contributing architecture diagrams.
* Documenting troubleshooting scenarios.
* Improving lab exercises and quizzes.
* Sharing lessons learned from different environments.

When contributing commands or configuration examples, include the IBM MQ version used for testing and the expected results whenever possible.

## References

* [IBM MQ Documentation](https://www.ibm.com/docs/en/ibm-mq)
* [IBM MQ 9.4 Documentation](https://www.ibm.com/docs/en/ibm-mq/9.4.x)
* [IBM MQ Clusters Documentation](https://www.ibm.com/docs/en/ibm-mq/9.4.x?topic=clusters)

---

**Goal:** Make IBM MQ clustering easier to understand, configure, test, and troubleshoot through practical, reproducible learning exercises.
