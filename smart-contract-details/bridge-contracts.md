---
icon: bridge-suspension
---

# Bridge Contracts

The bridge contracts will be deployed on all chains. On all chains they will be used to send or receive information needed to execute a specific function. Currently, we're planning to use the CCIP protocol for all bridging needs unless we have a reason to switch to another protocol.



Key Functions on Ethereum -

* Call the upgrade function on other supported chains (Arbitrum, XDC, etc.)
* Sync token information from/to other chains
* Select/Switch Sequencer
* Execute other emergency functions on other chains\


Key Functions on Local Chains -

* Pass on function calls from the L2 contracts to the Custom L2
* Pass on disputes and results of disputes\


Key Functions on OpenWork chain -

* Execute nativeDAO decision cross-chain, if needed.
* Sync token info
* Send rewards to other chains



Apart from the above specified functions, bridge contracts may be used to achieve other functionalities described on this doc.



\[highly optional] OpenWork’s seamless experience not needing transactions each time \[like DYDX]

We need to explore how to make the app easier to use without the user needing to make transactions each time for smaller operations like “change name” or “apply for a job”. Perhaps OpenWork can cover these fees or another solution should be explored for the same \[how DYDX does it perhaps]

\
\
