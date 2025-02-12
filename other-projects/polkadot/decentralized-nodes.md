# 💻 Decentralized nodes

{% hint style="info" %}
The 1KV program is ending and the Decentralized Nodes (DN) program is starting in its place. Currently, current 1KV participants have until October 31 to apply. The program will nominate about 75 Polkadot validators and about 180 Kusama validators for a period of 4 months. These numbers may change in the future due to changes in W3F stashes or due to changes in staking conditions
{% endhint %}

## Requirements

* Read the general requirements of the [Terms and Conditions](https://nodes.web3.foundation/terms) and the criteria of the [Rules](https://nodes.web3.foundation/rules)
* [KYC](https://in.sumsub.com/websdk/p/uni_pFuH7uRGbnHOmb6O) or [KYB](https://in.sumsub.com/idensic/l/#/uni_G31qZh1eO1RrbNFy) is required. Important - check the sanctions lists in the requirements. Russia and some other countries are prohibited
* Early 1KV participants will be given priority in the first cohort elections
* Need to start the node and configure the validator
* Fill out the [form](https://docs.google.com/forms/d/e/1FAIpQLSeUgcN2_qFjG-2bKY5Lfq8oJQt0wjO3hJt-jjdyBp8vyYcs8w/viewform) very carefully. The email in the form must match the one that has passed KYC
* Need to identity the main wallet that will be stashed. At a minimum, the Email and Matrix handle must be specified. Go to https://polkadot.polkassembly.io/ to complete verification of your Matrix descriptor ![](<../../.gitbook/assets/image (66).png>)
* Need own BOND tokens required for kusama 150 KSM and 7500 DOT for polkadot. If the operator applies for two Polkadot nodes or five Kusama nodes, the minimum stake per validator will be 6000 DOT and 100 KSM respectively
* A validator can charge a fee of up to 5% on Polkadot and up to 15% on Kusama
* The validator's reward assignment must be set to "staked" (increase its own stake). Alternatively, a validator can set the reward assignment parameter to "stash" if it has a stake of 11,500 DOT or 250 KSM. If an operator participates with two Polkadot nodes or five Kusama nodes, the above requirements become 10,000 DOT and 200 KSM per validator, respectively
* Nodes must be connected to a dedicated telemetry endpoint with a granularity level of 1:  `--telemetry-url "wss://telemetry-backend.w3f.community/submit/ 1"` If an operator wishes to make their setup public, they must specify this in their application. In this case, their node names will be published, and the command should be: `--telemetry-url "wss://telemetry-backend.w3f.community/submit/ 1" --telemetry-url "wss://telemetry.polkadot.io/submit/ 0"`
* No data should be hidden from telemetry. In particular, nodes should exchange at least the following information: (CPU, RAM, number of cores and whether the machine is virtual)
* The server should not use VPN
* Servers must meet the minimum hardware requirements described [here](https://wiki.polkadot.network/docs/maintain-guides-how-to-validate-polkadot#requirements)
* Each node must run on a separate server. There may be exceptions, but they are not recommended and must be justified
* Validators should never be slashed. The only exception is if the slashed was due to a client error or a general network problem
* Nodes must be updated to the latest client version within 24 hours of its release
* Validators must pay staking rewards no later than 24 hours after the end of the era
* You can't use Hetzner and Contabo
* Validators must be available to call (in English) upon request, during which they can be asked questions or asked to provide evidence of their transactions
* If a participant decides to leave the program, they must notify the Web3 Foundation via email at validators@web3.foundation at least one week prior to the node being disconnected
