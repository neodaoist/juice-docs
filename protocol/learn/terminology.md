---
description: >-
  The following terms are used frequently to describe ideas and mechanisms
  within the Juicebox protocol.
---

# Terminology

| Term                | Definition                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Project**         | <p>Each Juicebox project is represented as an NFT (ERC-721) who's owner is the address that makes the <code>launchProject</code> transaction on the <code>TerminalV2DataLayer</code> contract.<br><br>Ownership over this NFT permits admin-level interactions with the project. Like any other NFT, ownership can be transferred from the original owner to any other address, such as a multi-sig wallet.</p>                                                                                                                                                                                                                                                                                                                                                                                |
| **Funding cycle**   | A project's lifecycle is expressed in terms of funding cycles. A funding cycle is a data structure outlining the time-locked rules according to which a project wishes to operate.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| **Treasury tokens** | <p>The Juicebox protocol keeps track of treasury tokens for each project. When a payment is made to a project, the protocol mints tokens for a specified beneficiary according to the rules of the current funding cycle.</p><p>Projects can optionally call the <a href="../contracts/jbtokenstore/write/issuefor.md"><code>issueFor</code></a> transaction of the <a href="../contracts/jbtokenstore/"><code>JBTokenStore</code></a> contract to issue ERC-20 tokens to represent their treasury tokens. Once issued, anyone with a project's treasury tokens can claim them from the protocol's internal accounting mechanism into their wallet to use around Web3.</p><p>Treasury tokens can also be called Community tokens, or they may be referred to simply as a project's tokens.</p> |
| **Splits**          | A Split is a data structure used to send a percent of a total amount to a preprogrammed address, Juicebox project, or contract that inherits from `ISplitAllocator`. A split does not hold information about what is being split, it's simply a group-able structure that maps a receiver to a percentage.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| **Overflow**        | A funding cycle can have a `target` property which denotes how much funds a project can pull from its treasury during the cycle to distribute to its preprogrammed payout splits.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| **Discount rate**   | <p>Each funding cycle has a <code>weight</code>that is derived from multiplying the <code>weight</code> of the previous funding cycle by the <code>discountRate</code>of the previous cycle.<br><br>The <code>weight</code> property can then be used to determine how many tokens are issued per unit of payment received during the funding cycle, or for any other functionality you wish to implement through your funding cycle's data sources and delegates.</p>                                                                                                                                                                                                                                                                                                                         |
| **Redemption rate** | Each funding cycle has a `redemptionRate` property that can be used to determine how much overflowed funds can be claimed by redeeming/burning treasury tokens, or for any other functionality you wish to implement through your funding cycle's data sources and delegates.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| **Reserved tokens** | Each funding cycle has a `reservedRate`, which specifies the percentage of tokens minted as a result of newly received payments that should be reserved for distribution to a preprogrammed list of reserved token splits.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| **Ballot**          | Each funding cycle can include a ballot contract which specifies the conditions that must be met for any proposed funding cycle reconfiguration to take effect. A ballot contract can be written to incorporate strict community voting requirements in order to make funding cycle changes, or to simply add a required buffer period between when a change is proposed and when it can take effect.                                                                                                                                                                                                                                                                                                                                                                                          |
| **Operator**        | Addresses can give permissions to any other address to take specific actions throughout the Juicebox ecosystem on their behalf. These addresses are called Operators.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |