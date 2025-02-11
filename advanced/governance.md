# Governance

## Creating a New Proposal

Anybody can create a governance proposal which will start in the deposit period, and will be promoted to voting period once the minimum deposit amount is met. Anyone can deposit to a proposal in deposit period.

### [Submit Proposal](https://docs.cosmos.network/v0.45/modules/gov/07\_client.html#submit-proposal)

To submit a new proposal, you can send a transaction with the proposal details and a specified deposit amount. This deposit amount doesn't have to be greater than the MinDeposit (minimum to enter voting) amount, but until the overall deposit amount is met, the proposal will remain in deposit period.

{% hint style="warning" %}
A submit-proposal transaction must include a nonzero positive deposit amount
{% endhint %}

**Example**

```
seid tx gov submit-proposal param-change proposal.json --from {proposer_key}
```

Note that we allow for **expedited** proposals via the **--is-expedited** flag. This halves the time of the proposal but requires twice the amount of deposit.

### [Query Proposal](https://docs.cosmos.network/v0.45/modules/gov/07\_client.html#proposal)

You can also view existing proposal details and the state of the proposal (deposit period, voting period, etc) by querying for a specific proposal id.

**Example**

```
seid query gov proposal {proposal_id}
```

You can also query for the proposer for a specified proposal to view the address that initially submitted the proposal

**Example**

```
seid query gov proposer {proposal_id}
```

### [Deposit for Proposal](https://docs.cosmos.network/v0.45/modules/gov/07\_client.html#deposit)

If a created proposal is in a pending deposit period, you can add to the deposits in order to contribute for the proposal to enter the voting period. The deposit amount is denominated in amount to deposit and the deposit token such as `10000sei`.

{% hint style="danger" %}
If a proposal fails to meet MinDeposit before the deposit period ends, ALL deposits are burned
{% endhint %}

**Example**

```
seid tx gov deposit {proposal_id} {deposit_amount} --from {your_key}
```

### [Query Deposits](https://docs.cosmos.network/v0.45/modules/gov/07\_client.html#deposit)

A user can query the deposit made by a specific address on a specific proposal. This can be used to see your current deposit amount or to see the amount another account deposited.

**Example**

```
seid query gov deposit {proposal_id} {depositor_addr}
```

You can also query all deposits made for a proposal with a separate query command.

**Example**

```
seid query gov deposits {proposal_id}
```

## Voting on Proposals

This allows an address to vote on a specified proposal. There are four voting options when voting on a proposal

```
yes
no
abstain
no_with_veto
```

**Example**

```
seid tx gov vote {proposal_id} {vote_option} --from {voter_key} --chain-id {chain_id}
```

#### [Weighted Vote](https://docs.cosmos.network/main/modules/gov/07\_client.html#weighted-vote)

The weighted vote transaction allows a voter to partially allocate voting power to various voting options. This is especially useful in cases where the vote is voting on the behalf of multiple stakeholders with different voting decisions.

When performing a weighted vote, the transaction is executed with voting weights instead of a single option. The voting weights are expressed as a comma separated string of vote options mapping to voting weights. The voting weights must add up to 1 for the transaction to be valid.

**Voting Weights Example**

```
yes=0.3,no=0.2,no_with_veto=0.15,abstain=0.35
```

**Example**

```
seid tx gov weighted-vote {proposal_id} {voting_weights} --from {voter_key} --chain-id {chain_id} 
```

### Query

#### [Proposal Details](https://docs.cosmos.network/main/modules/gov/07\_client.html#proposal)

This will return the information about a single proposal specified by `proposal_id`.

**Example**

```
seid query gov proposal {proposal_id} --chain-id {chain_id}
```

#### [Proposal Tally](https://docs.cosmos.network/main/modules/gov/07\_client.html#tally)

This will return the current vote tally for the `proposal_id` provided.&#x20;

**Example**

```
seid query gov tally {proposal_id} --chain-id {chain_id}
```

#### [Proposal Votes](https://docs.cosmos.network/main/modules/gov/07\_client.html#votes)

This will return all the votes on the specified proposal.

**Example**

```
seid query gov votes {proposal_id} --chain-id {chain_id}
```

#### [Individual Vote](https://docs.cosmos.network/main/modules/gov/07\_client.html#vote)

This will query the vote information for a specific voter address and proposal id.

**Example**

```
seid query gov vote {proposal_id} {voter_addr} --chain-id {chain_id}
```
