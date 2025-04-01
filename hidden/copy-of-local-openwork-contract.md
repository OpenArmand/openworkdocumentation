---
hidden: true
icon: user-doctor
---

# Copy of Local OpenWork Contract

**The Local OpenWork Contract** on local chain just needs to have wrapper functions pointing to the corresponding native function on the Native OpenWork Contract through a bridge. The only exceptions being the functions where a payment is being locked or unlocked. Locking and unlocking of payments will happen on the Local chain. \
The below table lists all the functions to be included in this contract:

| Function                | Details                                                                                                                                                                                                                       |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| setProfile              | sets profile data by calling the corresponding function on the Native OpenWork Contract through the bridge contract                                                                                                           |
| getProfile              | gets profile hash by calling the corresponding function on the Native OpenWork Contract through the bridge contract                                                                                                           |
| postJob                 | posts the job on OpenWork by calling the corresponding function on the Native OpenWork Contract through the bridge contract                                                                                                   |
| applytoJob              | adds application to the job by calling the corresponding function on the Native OpenWork Contract through the bridge contract                                                                                                 |
| startJobInterChain      | This function will lock the specified amount on the local chain and then call the startJobInterChain function on the Native OpenWork contract                                                                                 |
| submitWork()            | This function will call the submitWork() function on the Native OpenWork Contract                                                                                                                                             |
| release\&LockNext()     | The payment will be released to the jobTaker and the payment for the next milestone locked on this contract(if last milestone, ignore locking step)  and then the release\&LockNextInterchain on the Native OpenWork contract |
| release\&TerminateJob() | The payment will be released on this contract and then the release\&terminateJobInterChain will be called on the Native OpenWork contract                                                                                     |

