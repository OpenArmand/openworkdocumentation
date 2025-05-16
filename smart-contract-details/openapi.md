---
icon: link
---

# OpenWork Chain

OpenWork Chain is a natively blockchain which will act as a single source of truth for the multi-chain OpenWork system. It is also intended to do the heavy lifting for the complex logic execution enabling other chains to offload their computation. This will reduce overall cost of execution.\
Deploying Athena on the OpenWork chain makes the dispute resolution process very affordable.  It can also be used a base-layer for future innovations. For eg., programmable work as outlined in [the whitepaper ](https://drive.google.com/file/d/1tdpuAM3UqiiP_TKJMa5bFtxOG4bU_6ts/view)or any other idea the community comes up with.

OpenWork Chain will be an optimistic roll-up based on the OP-Stack with the following customisations :

1. Genesis Block - The genesis block will be edited to include the following contracts&#x20;
   1. [Native OpenWork Contract](native-openwork-contract.md)
   2. [Earnings & Rewards Contract](rewards-tracking-and-payout-contracts.md)
   3. [Native DAO Contract ](openapi-2.md)
   4. [Native Athena Contract ](native-athena.md)
   5. [Bridge Contract](bridge-contracts.md)
2. The  configuration values will be tweaked to optimise the chain according to OpenWork’s requirements:&#x20;
   1. Validator Set :
      1. Set minimum stake of 0.3 ETH required to host a node.&#x20;
      2. Slashing Params (how much to penalise for wrong blocks & downtime).&#x20;
      3. Other applicable rules that help OpenWork stay decentralised.
   2. Transaction Fee Model - Make the transaction fee constant to a small amount.
3. The chain will interact with the Ethereum, XDC & Other L2s ( Arbitrum , Optimism , etc.) chains through a secure bridge, mostly Chain Link CCIP, unless a better option is discovered.
4. Some percentage (TBD) of the Custom L2 Transaction fee will go to the OpenWork Treasury.
5. Should accommodate all data needed in the application described above including OpenWork Ledger (OWL)

The code of the customised chain should be available on GitHub.

Once started, anyone in the OpenWork community can start a node by staking a pre-specified amount and cloning the OpenWork Chain GitHub repo.\


Validator/Node Operator Policy:

* Rewards: Combination of Transaction Fees + Small Block Reward (To be finalised at the time of development)
* Penalties: Proportional Slashing for Downtime and Fraud
* Un-bonding Period: 14 Days (To be finalised at the time of development)
* Maximum Validators: No Limits (To be finalised at the time of development)
* Penalties for fraudulent transactions : Validator should detect fraud transaction. For eg.  Fake jobs posted solely for the sake of earning rewards. If fraudulent transactions found
* All the above parameters in this policy should be upgradeable through a DAO proposal.
