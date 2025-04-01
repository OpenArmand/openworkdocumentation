---
icon: user-doctor
---

# Local OpenWork Contract

The Local OpenWork Contract acts as a wrapper on the local chain that directs calls to the corresponding functions on the Native OpenWork Contract through a bridge. The only exceptions are functions that handle payment locking/unlocking, which occur on the Local chain.

| **Function**                                     | **Implementation Details**                                                                                                                                                          |
| ------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| User should be able to set a profile             | <p><code>setProfile() {</code><br>Call <code>setProfile()</code> function on the Native OpenWork Contract through the bridge contract.<br>}</p>                                     |
| User should be able to retrieve a profile        | <p><code>getProfile() {</code><br>Calls <code>getProfile()</code> function on the Native OpenWork Contract through the bridge contract.<br>}</p>                                    |
| User should be able to post a Job                | <p><code>postJob() {</code><br>Calls <code>postJob()</code> function on the Native OpenWork Contract through the bridge contract<br>}</p>                                           |
| Ability to apply to a Job                        | <p><code>applyToJob() {</code><br>Call <code>applyToJob()</code> function on the Native OpenWork Contract through the bridge contract<br>}</p>                                      |
| Initiate a Job                                   | <p><code>startJobInterChain()</code> <strong>{</strong><br><code>LockPayment()</code><br> Call <code>startJobInterChain()</code> function on the Native OpenWork Contract.<br>}</p> |
| Selected Applicant should be able to submit work | <p><code>submitWork()</code> <strong>{</strong><br>Calls the <code>submitWork()</code> function on the Native OpenWork Contract.<br>}</p>                                           |
| Job Giver should be able to release payment      | <p><code>releasePayment() {</code></p><p><code>makePayment(jobTaker)</code></p><p>Call<code>releasePaymentInterchain</code> on the Native OpenWork Contract.<br>}</p>               |
| Job Giver should be able to lock next milestone  | <p><code>LockNextMilestone(){</code><br><code>LockPayment()</code> <br>Call <code>LockNextMilestoneInterchain</code> on the Native OpenWork Contract. <br>}</p>                     |
| Ratings                                          | <p><code>Rate() {</code> <br>Call <code>Rate()</code> on the Native OpenWork Contract. <br>}<br>Same goes for <code>getRating()</code></p>                                          |
| Portfolio Additions                              | <p><code>AddPortfolio() {</code><br>Call <code>AddPortfolio()</code> function on the Native OpenWork Contract<br>}</p>                                                              |

***
