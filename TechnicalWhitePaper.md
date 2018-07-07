# 858Chain Technical White Paper v1

**Jun 19, 2018**

**Abstract:** The 858Chain foundation introduces a new enterprise-grade hybrid blockchain platform designed to enable vertical and horizontal scaling of decentralized applications. This is archieved by creating an permissioned point to point chain system-like construct upon with applications can be built. the platform provides accounts, authentication, databases, asynchronous communication, and the scheduling of applications across many of CPU cores or clusters. The resulting technology is a blockchain architecture that may ultimately scale to millions of transactions per second, eliminates user fees, and allows for quick and easy deployment and maintenance of decentralized applications, in the context of a governed blockchain.

Copyright © 2018 858.capital 

Without permission, anyone may use, reproduce or distribute any material in this white paper for non-commercial and educational use (i.e., other than for a fee or for commercial purposes) provided that the original source and the applicable copyright notice are cited.

**DISCLAIMER:** This 858Chain Technical White Paper v1 is for information purposes only. 

<!-- MarkdownTOC depth=4 autolink=true bracket=round list_bullets="-*+" -->

- [Background](#background)
- [Requirements for Blockchain Applications](#requirements-for-blockchain-applications)
  - [Support Millions of Users](#support-millions-of-users)
  - [Free Usage](#free-usage)
  - [Easy Upgrades and Bug Recovery](#easy-upgrades-and-bug-recovery)
  - [Low Latency](#low-latency)
  - [Sequential Performance](#sequential-performance)
  - [Parallel Performance](#parallel-performance)
- [Consensus Algorithm \(BFT-DPOS\)](#consensus-algorithm-bft-dpos)
  - [Transaction Confirmation](#transaction-confirmation)
  - [Transaction as Proof of Stake \(TaPoS\)](#transaction-as-proof-of-stake-tapos)
- [Accounts](#accounts)
  - [Actions & Handlers](#actions--handlers)
  - [Role Based Permission Management](#role-based-permission-management)
    - [Named Permission Levels](#named-permission-levels)
    - [Permission Mapping](#permission-mapping)
    - [Evaluating Permissions](#evaluating-permissions)
      - [Default Permission Groups](#default-permission-groups)
      - [Parallel Evaluation of Permissions](#parallel-evaluation-of-permissions)
  - [Actions with Mandatory Delay](#actions-with-mandatory-delay)
  - [Recovery from Stolen Keys](#recovery-from-stolen-keys)
- [Deterministic Parallel Execution of Applications](#deterministic-parallel-execution-of-applications)
  - [Minimizing Communication Latency](#minimizing-communication-latency)
  - [Read-Only Action Handlers](#read-only-action-handlers)
  - [Atomic Transactions with Multiple Accounts](#atomic-transactions-with-multiple-accounts)
  - [Partial Evaluation of Blockchain State](#partial-evaluation-of-blockchain-state)
  - [Subjective Best Effort Scheduling](#subjective-best-effort-scheduling)
  - [Deferred Transactions](#deferred-transactions)
  - [Context Free Actions](#context-free-actions)
- [Token Model and Resource Usage](#token-model-and-resource-usage)
  - [Objective and Subjective Measurements](#objective-and-subjective-measurements)
  - [Receiver Pays](#receiver-pays)
  - [Delegating Capacity](#delegating-capacity)
  - [Separating Transaction costs from Token Value](#separating-transaction-costs-from-token-value)
  - [State Storage Costs](#state-storage-costs)
  - [Block Rewards](#block-rewards)
  - [Worker Proposal System](#worker-proposal-system)
- [Governance](#governance)
  - [Freezing Accounts](#freezing-accounts)
  - [Changing Account Code](#changing-account-code)
  - [Constitution](#constitution)
  - [Upgrading the Protocol & Constitution](#upgrading-the-protocol--constitution)
    - [Emergency Changes](#emergency-changes)
- [Scripts & Virtual Machines](#scripts--virtual-machines)
  - [Schema Defined Actions](#schema-defined-actions)
  - [Schema Defined Database](#schema-defined-database)
  - [Generic Multi Index Database API](#generic-multi-index-database-api)
  - [Separating Authentication from Application](#separating-authentication-from-application)
- [Inter Blockchain Communication](#inter-blockchain-communication)
  - [Merkle Proofs for Light Client Validation \(LCV\)](#merkle-proofs-for-light-client-validation-lcv)
  - [Latency of Interchain Communication](#latency-of-interchain-communication)
  - [Proof of Completeness](#proof-of-completeness)
  - [Segregated Witness](#segregated-witness)
- [Conclusion](#conclusion)

<!-- /MarkdownTOC -->

# Background

Hybrid blockchains are fairly unexplored and only few implementations exist; even in development. Quorum developed by JP Morgan is designed to be the hybrid blockchain in a fully permissioned environment. Truly Hybrid blockchain must necessarily be able to connect public blockchain with a private blockchain implementation running in a fully permissioned environment. The 858 hybrid blockchain aims to be exactly that, leveraging the power of both the public and private blockchain paradigms. 

Table 1 illustrates the comparison between public, private and hybrid blockchain systems

| Parameters               | Public Blockchain                 | Private Blockchain | Hybrid Blockchain                      |
| ------------------------ | --------------------------------- | ------------------ | -------------------------------------- |
| Transactional Visibility | Fully visible                     | Not visible        | Both; subject to use case              |
| Auditability             | Low                               | Low to High        | High                                   |
| Network                  | Decentralized                     | Centralized        | Hybrid                                 |
| Security                 | Compromised                       | High               | High                                   |
| Throughput               | Low(3-4/s Bitcoin; 15/s Ethereum) | Very high          | Very high                              |
| Membership               | Open to all                       | Restricted         | Participation Open, Hosting Restricted |
| Interoperability         | No                                | No                 | Yes                                    |

Table 1: Comparison of Blockchain platforms



