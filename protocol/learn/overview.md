---
description: >-
  Here you'll find information about how the Juicebox protocol is organized and
  implemented. All documentation in this section refers to the upcoming V2
  version of the protocol.
---

# Overview

The Juicebox protocol is made up of 7 core contracts (all documented), and 3 surface contracts (documentation WIP). Core contracts store all the independent components that make the protocol work, and the surface contracts glue them together while accept funds on behalf of each project.

The first two core contracts are pretty self explanatory. They store the core opinionated components of the protocol.&#x20;

* [`JBTokenStore`](../contracts/jbtokenstore/) manages token minting and burning for all projects.
* [`JBFundingCycleStore`](../contracts/jbfundingcyclestore/) manages funding cycle configurations, ballots, accounting, and scheduling. Funding cycles are represented as a [`JBFundingCycle`](../data-structures/jbfundingcycle.md) data structure.

The next few are a little more abstract. They don't know anything specific to Juicebox, and are open for use by other protocols or future Juicebox extensions.

*   [`JBProjects`](../contracts/jbprojects/) manages ownership over projects, which are represented as ERC-721 tokens.&#x20;

    The protocol uses these to enforce permissions to access several actions.&#x20;
*   [`JBSplitStore`](../contracts/jbsplitstore/) stores information about how arbitrary distributions should be split. The information is represented as a [`JBSplit`](../data-structures/jbsplit.md#jbsplit) data structure.&#x20;

    The protocol uses these to split up payout distributions and reserved token distributions.
*   [`JBPrices`](../contracts/jbprices/) manages and normalizes price feeds.&#x20;

    The protocol uses this to allow projects to do their accounting in any number of fiat currencies, but manage all funds in ETH.
*   [`JBOperatorStore`](../contracts/jboperatorstore/) stores operator permissions for all addresses. Addresses can give permissions to any other address to take specific indexed actions on their behalf, while confining the permissions to an arbitrary number of domains.&#x20;

    The protocol uses this to allow project owners to give other EOAs or contracts permission to take certain administrative actions on their behalf. This is useful for encouraging a composable ecosystem where proxy contracts can perform actions on a project's behalf as a lego block.

The last core contract stores info about which payment terminal's a project is currently accepting funds through, and which controller contract defines the rules according to which a project's terminal contract interacts with the core contracts.

* [`JBDirectory`](../contracts/jbdirectory/) keeps a reference of which terminal contracts each project is currently accepting funds through, and which controller contract is managing each project's tokens and funding cycles.

There are currently 3 surface contracts that manage how projects receive funds and define the rules for how all core contracts should be used together.

(The docs for these are WIP)

* [`JBController`](../contracts/or-controllers/jbcontroller/) stitches together funding cycles and treasury tokens, making sure all activity is accounted for and correct.
* [`JBETHPaymentTerminal`](../contracts/or-payment-terminals/jbethpaymentterminal/) manages all inflows and outflows of ETH into the Juicebox ecosystem.&#x20;
* [`JBETHPaymentTerminalStore`](../contracts/or-payment-terminals/jbethpaymentterminalstore/) manages balance accounting data on the `JBETHPaymentTerminal`'s behalf.

The `JBETHPaymentTerminal` inherits from the `IJBTerminal` interface. Projects are welcome to roll their own `IJBTerminal` to accept funds through. This can be useful to accept other tokens as payment, bypass protocol fees, or attempt some other funky design. A project can add/remove terminals using the [`JBDirectory.addTerminalOf(...)`](../contracts/jbdirectory/write/addterminalof.md) and [`JBDirectory.removeTerminalOf(...)`](../contracts/jbdirectory/write/removeterminalof.md).&#x20;

Likewise, a project can bring their own contract to serve as its controller. A project's controller is the only contract that has direct access to manipulate its tokens and funding cycles.