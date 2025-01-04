# 📨 Cosmos / governance

Cosmos is one of the few blockchains that really actively uses governance to discuss and make important decisions. Here it is worth mentioning right away that the proposal should not necessarily be directly related to any technical change in the blockchain - it can be a simple textual proposal in order to find out public opinion...

So, an proposal can be created (also make a deposit in the proposal) by any active blockchain user on the Cosmos hub who has coins in his wallet. Of course, if he has at least a small amount of coins for the initial deposit of the proposal

Only those who have linked (stake) their coins with validators from the active set can send their vote (vote and thereby increase the voting power). Creating a proposal does not guarantee that the proposal will enter the voting phase, as a mandatory deposit must be completed. There is also a chance that the deposit will be burned!!!

{% hint style="warning" %}
<mark style="color:red;">**Deposits are burned in the following cases:**</mark>

1. the minimum deposit is not typed
2. the proposal is vetoed (veto)
3. the quorum is not typed (quorum)
{% endhint %}

## Explanation of voting options

* **Abstain**: indicates that the voter does not intend to vote for or against the proposal, but accepts the outcome of the vote
* **Yes**: Indicates that the voter agrees with the proposal in its current form
* **No**: Indicates that the voter does not agree with the proposal in its current form
* **No With Veto**: Indicates a stronger rejection of the proposal than a simple No. If the number of No With Veto votes exceeds a third of the total number of votes, then the proposal is rejected and the deposits are burned

> <mark style="color:purple;">Sounds complicated at first glance, but let's try to figure it out</mark>

## Options

In order to create an proposal, we first need to know certain gov parameters, since these data may vary for different projects and of course they can also be changed through proposals

We will take the L1 blockchain as an example. And now that we have a starting point, we can find out the data we need in at least 2 ways:

**The first way is to open any explorer and see Governance Parameters**

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

**The second way through the CLI is to enter the command** `genesisd q gov params`

```shell
genesisd q gov params
deposit_params:
  max_deposit_period: "1209600000000000" # nanoseconds
  min_deposit:
  - amount: "10000000000000000000000" #  10 000 el1
    denom: el1
tally_params:
  quorum: "0.334000000000000000" # minimum voting power
  threshold: "0.500000000000000000" # minimum voting power to accept an
  veto_threshold: "0.334000000000000000" # minimum voting power to veto
voting_params:
  voting_period: "777600000000000" # nanoseconds
```

> <mark style="color:purple;">deposit\_params / tally\_params / voting\_params - we see the following subsections - let's go through each of them in more detail</mark>

## deposit\_params

<mark style="color:blue;">**max\_deposit\_period**</mark> - The maximum time an proposal can accept deposits before it expires

We see that in this network this parameter is 1209600000000000 nanoseconds, which is equal to 14 days. Accordingly, during this time it is necessary to collect the required amount of the deposit in order for the proposal to enter the voting period phase (voting\_period)

If, during this time, the required deposit amount is not reached, then any deposit amounts will be burned!!!

Decreasing this value will shorten the time that some proposals will be visible and increase the chance that some proposals will not reach the required amount and their deposits will be burned

<mark style="color:blue;">**min\_deposit**</mark> - the minimum deposit required for an proposal to enter the voting period phase (voting\_period)

We see that in this network this parameter is 10000000000000000000000 el 1, which equals 10,000 coins. Both the creator of the proposal and any other participant can make a deposit. Contributions of accepted and rejected proposals are returned to the authors

If the proposal is vetoed or the minimum deposit is not reached, then any deposit amounts will be burned!!!

Decreasing this value will allow you to create more proposals with fewer coins at risk of burning. But also a decrease can increase the number of proposals and lead to spam

## tally\_params

<mark style="color:blue;">**quorum**</mark> - the minimum share of network votes (voting power) required for the result of a governance proposal to be considered valid

We see that in this network this parameter is 0.334000000000000000, which is slightly more than 33.4% of the votes. A quorum is required for the voting results on the proposal to be valid and depositors to be able to return their deposited amounts. Voting power, whether a vote of 'yes', 'abstain', 'no', or 'no-with-veto', counts towards the quorum

If the vote does not reach a quorum, i.e. 33.4% voting power will not participate, then any deposit amounts will be burned and the result of the proposal will be considered invalid!!!

Reducing this value will allow a smaller part of the network to legitimize the outcome of the proposal. It also increases the risk that a decision will be made a smaller proportion of stakers positions, while reducing the risk that the proposal will be invalidated. And it will probably reduce the risk of burning deposits

<mark style="color:blue;">**threshold**</mark> - the minimum share of voting power required to accept an proposal

We see that in this network this parameter is 0.500000000000000000, which equals 50% voting power. So to accept the proposal, at least 50% of the voting power must vote 'yes'. Keep in mind that a simple majority vote of those who voted 'yes' may not be enough to accept a proposal due to:

* not reaching a quorum of 33.4%
* 'no-with-veto' vote is 33.4% or more of member votes

If the management proposal is accepted, the deposit amounts are returned to the contributors. If the text proposal passes, nothing is automatically set in motion, but there is a public expectation that the participants will coordinate actions to accept the obligations outlined in the proposal

If the proposal to change the parameters is accepted, the protocol parameter will automatically change immediately after the end of the voting period. If the community spending proposal is accepted, the community pool balance will decrease by the number of atoms specified in the proposal, and the recipient address will increase by the same number of atoms immediately after the end of the voting period

Decreasing this value will increase the likelihood that the proposal will be accepted and a small group will be able to make a change to the network

<mark style="color:blue;">**veto\_threshold**</mark> - minimum voting power share for veto power (i.e. fail)

We see that in this network this parameter is 0.334000000000000000, which equals 33.4% of the voting power. Even if more than 50% of the voting power votes for the 'yes' proposal, then 33.4% of the voting power who voted 'no with-veto' will be able to override this decision. This allows a minority group representing more than 1/3 of the vote to reject a proposal that might otherwise pass

If vetoed, any deposit amounts will be burned!!!

Decreasing this value will allow a smaller group to block controversial proposals

## voting\_params

<mark style="color:blue;">**voting\_period**</mark> - The maximum time that an proposal can be voted on. Voters may change their vote any number of times before the end of the voting period. Voting period should always be shorter than Unbonding period to prevent double voting

We see that in this network this parameter is 777600000000000 nanoseconds, which is equal to 9 days

If the vote does not reach a quorum before this time, i.e. 33.4% of the network's vote will not participate, then any deposit amounts will be burned and the proposal result will be considered invalid!!!

Decreasing this value will reduce the voting time, which in turn will reduce the likelihood that a quorum will be reached

{% hint style="info" %}
### Inheritance

* If the delegator does not vote, then it will inherit the vote of its validator
* If the delegate votes before his validator, he will not inherit the validator's votes
* If the delegate votes after his validator, he will override the validator's vote with his own
{% endhint %}



## Useful commands

```
# list of proposals
genesisd q gov proposals

# view vote result
genesisd q gov proposals --voter <ADDRESS>

# vote for proposal
genesisd tx gov vote 1 yes --from <name_wallet> --fees 555el1

# create a simple text proposal
genesisd tx gov submit-proposal --title="Randomly reward" --description="Reward 10 testnet participants who completed more than 3 tasks" --type="Text" --deposit="11000000el1" --from=<name_wallet> --fees 500el1

# make a deposit to the proposal
genesisd tx gov deposit <_proposals> 1000000el1 --from <name_wallet> --fees 555el1

# create a proposal with execution
# the first stage is creating a file.json with the necessary content, for example
nano /root/.genesisd/genesis-validator-gift-proposal.json
{
  "title": "<Genesis Gift all validators>",
  "description": "<Peace to the whole world>",
  "recipient": "<genesis1a...>",
  "amount": "<50000000000000000000el1>",
  "deposit": "<1000000000000000000el1>"
}
# second stage send transaction
genesisd tx gov submit-proposal community-pool-spend /root/.genesisd/genesis-validator-gift-proposal.json --from=<name_wallet> --fees 555el1
```



Useful links:

* [https://github.com/cosmos/governance/blob/master/overview.md](https://github.com/cosmos/governance/blob/master/overview.md)
* [https://github.com/cosmos/cosmos-sdk/blob/main/x/gov/spec/01\_concepts.md#deposit-refund-and-burn](https://github.com/cosmos/cosmos-sdk/blob/main/x/gov/spec/01_concepts.md#deposit-refund-and-burn)
* [https://hub.cosmos.network/main/governance/](https://hub.cosmos.network/main/governance/)
