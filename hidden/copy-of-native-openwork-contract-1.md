---
hidden: true
icon: user-doctor
---

# Copy of Native OpenWork Contract

**The Native OpenWork Contract** on OW chain needs to be in sync with the following:

1. It should be compatible with all relevant features described in the UI description\[see below table for details].
2. It should accommodate dispute resolution through Athena
3. It should be made upgradable so it’s compatible with future needs for programming jobs and making assessments based on other factors other than disputes.

The below table describes all the user facing functions that the contract will have. Note that there might be some internal functions needed apart from this.

| Functions                                                                                                                                               | Details                                                                                                                                                                                                                          |
| ------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>setProfile<br>getProfile</p>                                                                                                                         | <p>interacts with the Profile Struct(detailed below)</p><p>[Profile</p><p>{address, ipfs_hash, referrer_address, rating}</p><p>]</p>                                                                                             |
| postJob / getJobDetailByID / getAllJobIDs / getTotalJobValue / getMilestonePayments / getCurrentMilestone                                               | <p>Interacts with the Job Struct (detailed below)</p><p>[Job</p><p>{id, jobGiver, applicants:[], jobDetailHash, isOpen, workSubmissions:[], milestonePayments:[], totalPaid, currentLockedAmount, currentMilestone }</p><p>]</p> |
| <p>applyToJob /<br>getApplicationByID</p>                                                                                                               | <p>Interacts with the Application Struct (detailed below)</p><p>[Application<br>{id, applicant, hash, proposedMilestones, isSelected}<br>]</p>                                                                                   |
| <p>initiateJob(&#x26; lock 1st Milestone) / </p><p>initiateInterChainJob (without locking amount) /</p><p>getSelectedApplicant /<br>getAmountLocked</p> | Interacts with the Job Struct                                                                                                                                                                                                    |
| <p>submitWork /<br>getSubmissionByID</p>                                                                                                                | <p>Interacts with the workSubmission Struct (detailed below)</p><p>[workSubmission </p><p>{ id, applicant, hash }</p><p>]</p>                                                                                                    |
| releasePayment(& lock next milestone) / getCurrentMilestone                                                                                             | Interacts with the Job Struct and the Earnings & Rewards Contract to determine if rewards are applicable                                                                                                                         |
| Rate / getRating                                                                                                                                        | Interacts with the Profile Struct                                                                                                                                                                                                |
| raiseDispute / getDisputeDetails                                                                                                                        | Interacts with the Native Athena Contract                                                                                                                                                                                        |
| claimDisputedAmount / getDisputeWinner                                                                                                                  | Calls the Athena Contract                                                                                                                                                                                                        |
| Upgrade                                                                                                                                                 | Upgrades the implementation logic of the contract itself                                                                                                                                                                         |
