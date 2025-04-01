---
hidden: true
icon: network-wired
---

# Old Native DAO Contract

The Native DAO on the OpenWork chain should facilitate the following:

1. Athena’s Governance: The nativeDAO should have the ability to do the following based on the same. 80% majority approval with 20% token votes. All rules here must be upgradable:
2. Add and remove “skills” on OpenWork.\[bulk]
3. Creation of a general skill oracle \[this will happen through same process]. &#x20;
4. Creation of skill oracles for skills added. .\[bulk]
5. Making a member eligible to stake and join a skill oracle. Also removing members. \[bulk]
6. 100k tokens need to be locked/staked for Skill Oracle membership. Same condition applies for initiating Athena related proposals&#x20;
7. Once 20 active members are part of a skill oracle, the skill oracle will be marked as active. Once active it itself can add or remove members and also function for dispute resolution. Active would be defined initially as those who have participated in voting as part of the oracle (including marking themselves active) for the last 30 days (or another parameter that makes sense). For now, it’s just if 20 are part of the oracle.
8. Upgrade the governance of the skill oracles.
9. Remove stake of member as a penalty
10. Other compatible functions for Athena
11. Incentive for skill oracles&#x20;
12. Potentially in the future if not now, the ability to perform super-admin actions like resolving disputes, assigning skills, overruling the skill oracle.&#x20;
13. Should interface with the MainDAO to validate things like stakes tokens and other essential data.

\


Old DAO contracts&#x20;

Here is the Old unfinished OpenWorkDAO (with a child contract to manage the DAO that initiates Athena after it)

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts-upgradeable/token/ERC20/IERC20Upgradeable.sol";
import "@openzeppelin/contracts-upgradeable/token/ERC20/utils/SafeERC20Upgradeable.sol";
import "@openzeppelin/contracts-upgradeable/governance/GovernorUpgradeable.sol";
import "@openzeppelin/contracts-upgradeable/governance/extensions/GovernorSettingsUpgradeable.sol";
import "@openzeppelin/contracts-upgradeable/governance/extensions/GovernorCountingSimpleUpgradeable.sol";
import "@openzeppelin/contracts-upgradeable/governance/extensions/GovernorVotesUpgradeable.sol";
import "@openzeppelin/contracts-upgradeable/governance/extensions/GovernorVotesQuorumFractionUpgradeable.sol";
import "@openzeppelin/contracts-upgradeable/governance/extensions/GovernorTimelockControlUpgradeable.sol";
import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";
import "@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol";
import "@openzeppelin/contracts-upgradeable/proxy/utils/UUPSUpgradeable.sol";
import "@openzeppelin/contracts-upgradeable/security/ReentrancyGuardUpgradeable.sol";

contract OpenWorkDAO is Initializable, GovernorUpgradeable, GovernorSettingsUpgradeable, GovernorCountingSimpleUpgradeable, GovernorVotesUpgradeable, GovernorVotesQuorumFractionUpgradeable, GovernorTimelockControlUpgradeable, OwnableUpgradeable, UUPSUpgradeable, ReentrancyGuardUpgradeable {
    using SafeERC20Upgradeable for IERC20Upgradeable;

    IERC20Upgradeable public token;
    uint256 public constant LOCK_PERIOD = 365 days;

    struct Stake {
        uint256 amount;
        uint256 startTime;
    }

    mapping(address => Stake) public stakes;

    function initialize(IVotes _token, TimelockControllerUpgradeable _timelock, address initialOwner)
        initializer public
    {
        __Governor_init("OpenWorkDAO");
        __GovernorSettings_init(7200 /* 1 day */, 50400 /* 1 week */, 1000000e18);
        __GovernorCountingSimple_init();
        __GovernorVotes_init(_token);
        __GovernorVotesQuorumFraction_init(30);
        __GovernorTimelockControl_init(_timelock);
        __Ownable_init();
        transferOwnership(initialOwner);
        __UUPSUpgradeable_init();
        token = IERC20Upgradeable(address(_token));
    }

    function stake(uint256 amount) public nonReentrant {
        token.safeTransferFrom(msg.sender, address(this), amount);
        stakes[msg.sender] = Stake({
            amount: stakes[msg.sender].amount + amount,
            startTime: block.timestamp
        });
    }

    function unstake(uint256 amount) public nonReentrant {
        require(stakes[msg.sender].amount >= amount, "Not enough staked");
        require(block.timestamp >= stakes[msg.sender].startTime + LOCK_PERIOD, "Stake is locked");
        stakes[msg.sender].amount -= amount;
        token.safeTransfer(msg.sender, amount);
    }

    function votingPower(address voter) public view override returns (uint256) {
        if (block.timestamp >= stakes[voter].startTime + LOCK_PERIOD) {
            return stakes[voter].amount;
        }
        return 0;
    }

    function _voteSucceeded(uint256 proposalId) internal view override returns (bool) {
        uint256 votesInFavor = proposalSnapshot(proposalId).forVotes;
        uint256 totalVotes = proposalSnapshot(proposalId).forVotes + proposalSnapshot(proposalId).againstVotes + proposalSnapshot(proposalId).abstainVotes;
        return (votesInFavor * 100) / totalVotes >= 80;
    }

    function transferERC20(address tokenAddress, address to, uint256 amount) public onlyGovernance {
        IERC20Upgradeable(tokenAddress).safeTransfer(to, amount);
    }

    function receiveERC20(address tokenAddress, uint256 amount) public {
        IERC20Upgradeable(tokenAddress).safeTransferFrom(msg.sender, address(this), amount);
    }

    // Override functions for governance controls
    function votingDelay() public view override(GovernorUpgradeable, GovernorSettingsUpgradeable) returns (uint256) {
        return super.votingDelay();
    }

    function votingPeriod() public view override(GovernorUpgradeable, GovernorSettingsUpgradeable) returns (uint256) {
        return super.votingPeriod();
    }

    function quorum(uint256 blockNumber) public view override(GovernorUpgradeable, GovernorVotesQuorumFractionUpgradeable) returns (uint256) {
        return super.quorum(blockNumber);
    }

    function proposalThreshold() public view override(GovernorUpgradeable, GovernorSettingsUpgradeable) returns (uint256) {
        return super.proposalThreshold();
    }

    function _authorizeUpgrade(address newImplementation) internal override onlyOwner {}
}
// note that where “onlyOwner” is, it should probably be the governor function allowing the DAO to vote and agree on an update


```

OpenWork DAOAthena Contract (unfinished)

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts-upgradeable/token/ERC20/IERC20Upgradeable.sol";
import "@openzeppelin/contracts-upgradeable/token/ERC20/utils/SafeERC20Upgradeable.sol";
import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";
import "@openzeppelin/contracts-upgradeable/proxy/utils/UUPSUpgradeable.sol";
import "@openzeppelin/contracts-upgradeable/utils/structs/EnumerableSetUpgradeable.sol";

contract OpenWorkDAOAthena is Initializable, UUPSUpgradeable {
    using SafeERC20Upgradeable for IERC20Upgradeable;
    using EnumerableSetUpgradeable for EnumerableSetUpgradeable.AddressSet;

    IERC20Upgradeable public openWorkToken;
    uint256 public minStake;
    uint256 public minMembers;
    uint256 public constant ENTRY_STAKE = 1e6 * 1e18; // 1 million tokens, assuming 18 decimal places
    uint256 public constant MIN_VOTING_DURATION = 4 days; // Minimum voting duration

    struct Member {
        mapping(string => uint256) stakedTokens; // Tokens staked per oracle
    }

    struct Oracle {
        bool exists;
        bool isActive;
        EnumerableSetUpgradeable.AddressSet potentialMembers;
        EnumerableSetUpgradeable.AddressSet members;
        uint256 activationTimestamp;
        Proposal[] proposals;
        mapping(address => uint256) votingPower; // Voting power per member within this oracle
    }

    struct Proposal {
        uint256 id;
        string description;
        uint256 votesFor;
        uint256 votesAgainst;
        uint256 startTime; // Start time of the voting period
        bool executed;
        address candidate; // Candidate address for proposals
        string skill; // Skill information for skill verification
    }

    mapping(string => Oracle) public oracles; // All oracles [string is skill]
    mapping(address => Member) public members; // All members

    function initialize(IERC20Upgradeable _openWorkToken, uint256 _minStake, uint256 _minMembers) public initializer {
        __UUPSUpgradeable_init();
        openWorkToken = _openWorkToken;
        minStake = _minStake;
        minMembers = _minMembers;
    }

    function _authorizeUpgrade(address newImplementation) internal override onlyGovernance {}

    function createOracle(string memory oracleName) public {
        require(!oracles[oracleName].exists, "Oracle already exists");
        Oracle storage oracle = oracles[oracleName];
        oracle.exists = true;
        oracle.isActive = false;
        oracle.activationTimestamp = 0; // Initially inactive
    }

    function activateOracle(string memory oracleName) public { //maybe instead of public, it needs a governance only function
        require(oracles[oracleName].exists, "Oracle does not exist");
        require(!oracles[oracleName].isActive, "Oracle already active");
        oracles[oracleName].isActive = true;
        oracles[oracleName].activationTimestamp = block.timestamp;
    }

    function deactivateOracle(string memory oracleName) public {
        require(oracles[oracleName].exists, "Oracle does not exist");
        require(oracles[oracleName].isActive, "Oracle is not active");
        oracles[oracleName].isActive = false;
    }

    function addMember(string memory oracleName, address member) public {
        require(oracles[oracleName].isActive, "Oracle is not active");
        oracles[oracleName].members.add(member);
    }
//this should potentially be the governance making a member eligible (through potential member), then this is checked once they stake

    function removeMember(string memory oracleName, address member) public {
        require(oracles[oracleName].isActive, "Oracle is not active");
        oracles[oracleName].members.remove(member);
    }

    function stakeTokens(string memory oracleName, uint256 amount) public {
        require(oracles[oracleName].exists, "Oracle does not exist");
        require(amount >= ENTRY_STAKE, "Minimum stake not met");
        openWorkToken.safeTransferFrom(msg.sender, address(this), amount);
        members[msg.sender].stakedTokens[oracleName] += amount;
        oracles[oracleName].votingPower[msg.sender] += amount;
    }
//this should potentially be the the member going from the potential member list to actual member, given they’re eligible and staked now.


    function withdrawTokens(string memory oracleName, uint256 amount) public {
        require(oracles[oracleName].exists, "Oracle does not exist");
        Member storage member = members[msg.sender];
        require(member.stakedTokens[oracleName] >= amount, "Insufficient staked tokens");
        openWorkToken.safeTransfer(msg.sender, amount);
        member.stakedTokens[oracleName] -= amount;
        oracles[oracleName].votingPower[msg.sender] -= amount;
    }

    function createProposal(string memory oracleName, string memory description) public returns (uint256) {
        require(oracles[oracleName].isActive, "Oracle is not active");
        require(oracles[oracleName].members.contains(msg.sender), "Only members can propose");
        require(members[msg.sender].stakedTokens[oracleName] >= ENTRY_STAKE, "Insufficient stake");

        Oracle storage oracle = oracles[oracleName];
        uint256 proposalId = oracle.proposals.length;
        oracle.proposals.push(Proposal({
            id: proposalId,
            description: description,
            votesFor: 0,
            votesAgainst: 0,
            startTime: block.timestamp,
            executed: false,
            candidate: address(0), // Default to zero address
            skill: "" // Default to empty string
        }));
        return proposalId;
    }

    function voteOnProposal(string memory oracleName, uint256 proposalId, bool support) public {
        Oracle storage oracle = oracles[oracleName];
        Proposal storage proposal = oracle.proposals[proposalId];

        require(block.timestamp <= proposal.startTime + MIN_VOTING_DURATION, "Voting period has ended");
        require(!proposal.executed, "Proposal already executed");
        uint256 voterPower = oracle.votingPower[msg.sender];
        require(voterPower > 0, "No voting power");

        if (support) {
            proposal.votesFor += voterPower;
        } else {
            proposal.votesAgainst += voterPower;
        }
    }

    function executeProposal(string memory oracleName, uint256 proposalId) public {
        Oracle storage oracle = oracles[oracleName];
        Proposal storage proposal = oracle.proposals[proposalId];

        require(!proposal.executed, "Proposal already executed");
        require(block.timestamp >= proposal.startTime + MIN_VOTING_DURATION, "Minimum voting duration has not elapsed");
        require(proposal.votesFor > proposal.votesAgainst, "Proposal did not pass");

        proposal.executed = true; // Mark the proposal as executed
    }
}
```

\
\


