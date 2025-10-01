---
description: >-
  OpenWork's instant cross-chain USDC payment infrastructure using Circle's CCTP
  for sub-1000ms transfers.
icon: transgender
---

# CCTP Transceiver

System Integration

* Job Payments: Enables instant milestone payments between chains (e.g., job giver on OP → job taker on Arbitrum)
* Dispute Fees: Routes dispute fees from local Athena Clients to Native Athena on Arbitrum
* LOWJC Integration: Used by local job contracts for cross-chain payment escrow release

Key Features

* Fast Transfers: Sub-1000ms settlement via CCTP v2's burn-and-mint mechanism
* Domain Mapping: Converts LayerZero chain IDs to CCTP domains for payment routing
* Address Conversion: Handles Ethereum address → bytes32 conversion for CCTP compatibility

Use Cases

1. Cross-chain job milestone payments
2. Instant dispute fee routing to Native Athena
3. Platform liquidity management between chains
