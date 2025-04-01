---
hidden: true
icon: sack-dollar
---

# Copy of Earnings and Reward Contract

Earnings and Rewards Contract on the OpenWork Chain should be compatible with the below points:

1. Earnings & Governance Rewards are given at the completion of each job based on the ratios related to the total job value on OpenWork.  **Note that pricing of the token would need to be gotten from a source like a uniswap pool** \[potentially it could be self determined but this could be dangerous and **needs to be explored**].
2. Team tokens are equal to pre-staked tokens \[1 year, 2 year and 3 year staking done with these tokens when given, for ease of the system]. These tokens are awarded to the Team but can’t be accessed(i.e. seen in their wallet or transferred) until necessary governance is done.&#x20;
3. Initial Tokens Must be Vested/Staked in a Safe Way where the DAO can remove a member’s tokens if required due to malicious action, or not fulfilling promises.
4. Tokens are issued upon job completion based on cumulative platform value:

* < $1M: 100% of job value
* $1M–$10M: 50% of job value
* $10M–$100M: 25% of job value
* $100M–$1B: 10% of job value

4. If a referrer exists, 10% of tokens go to the referrer and 90% to the job giver; otherwise, the job giver receives 100%.
5. A job taker’s  referrer also earns 10% of the fee.
6. People with earned tokens can redeem 10k tokens per governance action.&#x20;
7. Tokens are claimable once staking periods are done, or can be re-staked.
8. Further, job givers can allocate a percentage of some tokens to the job takers based on their agreements with them \[of the pool given to them].

\


| Type of Token           | Stake Period                                     | Pre-Staked? | Governance Action Required                       | Governance Power                        |
| ----------------------- | ------------------------------------------------ | ----------- | ------------------------------------------------ | --------------------------------------- |
| Earned Tokens           | 1 year (pre-staked each time they're earned)     | Yes         | Yes (e.g., 1 action per 10,000 tokens to unlock) | 1x                                      |
| Team Tokens             | 1/3 for 1 year, 1/3 for 2 years, 1/3 for 3 years | Yes         | Yes (e.g., 1 action per 10,000 tokens)           | 1x, 2x, or 3x based on staking duration |
| Voluntary Staked Tokens | 1, 2, or 3 years (user choice)                   | No          | No                                               | 1x, 2x, or 3x based on duration         |

The Earnings and Reward Contract on the OpenWork Chain will calculate and store the rewards for all users on the OW chain but the rewards will be claimed through the Main DAO contract on Ethereum after the necessary governance is done to redeem the tokens after the staking period.\
\
