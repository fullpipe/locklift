# Concept of Transaction Finalization

In the world of blockchain technology, the concept of transaction finalization is pivotal. It refers to the process of confirming and validating transactions within the network, ensuring their immutability and integration into the blockchain. This document aims to delve deeper into the specifics of transaction finalization in the context of TVM-compatible blockchains, which operate based on a pure actor model.

## Actor Model & Async Transactions

TVM-compatible blockchains operate based on a pure actor model, an asynchronous and concurrent computational model. In this system, each contract acts as an independent actor, processing its own messages. Hence, the execution of a smart contract or transaction is not a single, atomic operation. Instead, it's a sequence of actions that can happen asynchronously.

For instance, let's imagine `Contract A` calling `Contract B`. In many other blockchain models, if `Contract B` runs out of gas during execution, the entire transaction would be rolled back, including the state changes in `Contract A` that occurred prior to the call. However, in the actor model of TVM blockchains, if `Contract B` encounters an error, the changes `Contract A` made before calling `Contract B` would persist. The execution failure of `Contract B` only rolls back changes related to `Contract B`'s execution. This characteristic ensures a higher degree of reliability and security in transaction processing.

## Transaction Flows

Transactions in these blockchains are part of a continuous, asynchronous process where numerous actors interact, process messages, and modify states. Each smart contract (actor) follows a specific behavior pattern, responding to events, executing its code, modifying its own properties, optionally generating outgoing messages, and then going into standby mode until the next event occurs. These sequences form an `AccountChain`, a chain of transactions for a single account, which is then included in a block without disrupting the sequencing.

When considering multiple accounts, we have multiple `AccountChains` forming a `ShardChain`, which can be dynamically split and merged depending on the transaction load. Ultimately, all shards that contain all accounts following a set of rules form a Blockchain, where multiple blockchains can operate simultaneously and interact with each other.

## Logical Time in Transactions

Every transaction within the TVM-compatible blockchain is assigned a logical time interval. This logical time serves as a unique identifier for transactions and outbound messages of an account. The logical time intervals of transactions of the same account do not intersect each other, thus ensuring that all outbound messages generated by an account are unique and identifiable.

## Components of a Transaction

Each transaction within the TVM-compatible blockchain contains or indirectly refers to the following data:

- The account to which the transaction belongs.
- The logical time of the transaction.
- An inbound message processed by the transaction. Each transaction is always initiated by this message.
- The number of generated outbound messages.
- The outbound messages themselves.
- The initial and final state of the account, including its balance.
- The total fees collected by the validators.
- A detailed description of the transaction containing all or some data needed to validate it.

## Types of Transactions

There are different types of transactions allowed in the blockchain, each serving a unique purpose:

- **Ordinary transactions:** These belong to an account and process exactly one inbound message, compute the new state of the account, and generate several outbound messages.
- **Storage transactions:** These transactions do not process any inbound message and do not invoke any code. Their only effect is to collect storage payments from an account, affecting its storage statistics and its balance.
- **Tick and Tock transactions:** These are automatically invoked for certain special accounts in the masterchain that have the tick flag set in their state, as the very first transactions in every masterchain block or the very last transactions in every masterchain block.
- **Split and Merge transactions:** These are invoked as the last transactions of shardchain blocks immediately preceding a shardchain split event or immediately after a shardchain merge event, if an instance of a large smart contract needs to be merged with another instance of the same smart contract.

## Transaction Processing Phases

The processing of an inbound message is split into two phases: the computing phase and the action phase. During the computing phase, the virtual machine is invoked and the necessary computations are performed, but no actions outside the virtual machine are taken. The actions themselves are postponed until the action phase, during which the user smart contract is not invoked at all.

:::tip
To gain a deeper understanding of the Compute and Action phases, please refer to our detailed guide [here](./compute-action-phases.md).
:::