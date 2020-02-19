# Intro

This past weekend I participated in the Eth Denver Hackathon. Together, my team and I came up with the idea to build "MetaCredits", a smart contract designed to automatically fund the gas to execute metatransactions. Metatransactions are unique from regular transactions in that the user who is creating the transaction (signed data) is not posting it as a transaction to the blockchain, instead sending the message to a relayer service which will pay for the gas to execute the transaction that the user signed. For more about metatransactions check out [Austin Griffith's medium post](https://medium.com/@austin_48503/ethereum-meta-transactions-90ccf0859e84).

## Design Rationale

Metatransactions are super useful because they allow Dapp developers to pay for their users transactions, lowering the barriers to end users interacting with their Dapp and the ethereum mainnet blockchain. Now Dapp users do not have to acquire eth necessary to pay transaction fees, the Dapp developer can choose to pay this gas cost while still allowing the user to sign the transaction and ensure that the system remains decentralized.

But what if a Dapp Developer does not have access to ether? Our solution aims to build a bridge to these Dapp developers who may not be able to afford an amount of ethereum they need to fun their Dapp or who may live in a country with strict KYC laws and are not able to exchange their currency for ether. These developers can now request to be funded via the MetaCredits smart contract and benefactors with the means to pay the fees for these bootstrapping transactions can safely fund the developers via MetaCredits.

A small amount of gas can go a long way...the 0.02 eth that you spent on `helloworld.eth` ENS domain could pay for a number of small transactions on a smart contract. We believe that there is enough goodwill and benevolence across the ethereum ecosystem that this model would be very helpful in bringing needy dapp developers together with benefactors/sponsors who are willing to help get their app off the ground with minimal costs.

## Technical Components

[pic here]

There are a number of modular components in the MetaCredits solution.

1. Developer Dapp
The developer creates a metatransaction enabled smart contract and builds a Dapp. Without ether, they can't deploy their dapp to mainnet, but they can acquire free testnet ether and test out their dapps on one of the test networks.

2. Dapp Approver Microservice
The Dapp developer can run this tiny lightweight service to verify that the signed metatransaction from his end users match the parameters of transactions he is willing to fund.

3. Relayer Microservice
This service contains a funded wallet which will execute the transaction by calling a function on the MetaCredit Funder Contract.

4. MetaCredit Funder Contract
This contract is funded by a benefactor and the funds inside will be used to repay relayers in full for the gas they spend to execute the metatransaction.