---
icon: cloud
---

# What gets stored in IPFS?

All data that is not essential to the execution of the Smart Contract functions gets pinned on IPFS before calling a Smart Contract function and only the hash of that data is stored in the smart contract for reference.

The detaols\
\
profileDetails {\
username, firstName, lastName, email, referrerAddress, location, experience, perHourRate, description, phoneNumber, skills, languages\
}\
\
jobDetails { title, description, skillsNeeded, files, oracleName,  totalCompensation, milestones: \[milestone1 : {title, details, date, amount}, milestone2, milestone3, milestone4.., jobGiverAddress, selectedApplicantAddress, applications:\[]]\
}\
\
jobApplication {\
description, files,  totalCompensation, milestones: \[milestone1 : {title, details, date, amount}, milestone2, milestone3, milestone4..]\
}

workSubmission {\
updateTitle, updateDescription, files,}\
\
disputeDetails {\
disputeTitle, disputeExplanation, disputedAmount, oracleFee, disputeRaisedBy, disputeRaisedAgainst, disputeFiles }

portfolio {\[ project1: { title, description, tags, imageFile, link}, project2: ...and so on]}\
\
skillApplication {\
appliedBy, oracleName, description, files, fee}\
\
athenaRecruitmentProposal {\
memberToAdd, oracleName, description}\
\
oracleApplication {\
oracleName, description, files}\
\
servicePackageDetails { amount, title, details, files, tags, link}\
\
