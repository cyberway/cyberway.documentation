# Core Concepts

## Accounts, Wallets and Permissions

#### Accounts

An account is a human-readable name that is stored on the blockchain. It can be owned through authorization by an individual or group of individuals depending on permissions configuration. An account is required to transfer or push any valid transaction to the blockchain.

#### Wallets

Wallets are clients that store keys (only keys, not tokens) that may or may not be associated with the permissions of one or more accounts. Ideally, a wallet has a locked (encrypted) and unlocked (decrypted) state that is protected by a high entropy password. The CyberWay repository comes bundled with a CLI client called `cleos` that interfaces with a lite-client called `keosd` and together, they demonstrate this pattern.

#### Authorization and Permissions

Permissions are arbitrary names used to define the requirements for a transaction sent on behalf of that permission. Permissions can be assigned for authority over specific contract actions by linking authorization or linkauth.

For more information about these concepts, see the [Accounts and Permissions](!!!!) documentation.

## Smart Contracts
A smart contract is a piece of program code that can executed on a blockchain and keep the state of contract execution as a part of the immutable history of that blockchain instance. It is a self-executing set of functions (algorithms) with the terms of the agreement between parties being directly written into lines of code. The code and the agreements contained therein exist across a distributed, decentralized blockchain network. The code controls the execution, and transactions are trackable and irreversible. Developers can rely on that blockchain as a trusted computation environment in which inputs, execution, and the results of a smart contract are independent and free of external influence.

More details about smart contracts being used on CyberWay can be found [here](https://docs.cyberway.io/devportal/system_contracts).


## Delegated Proof of Stake (DPOS)
The CyberWay platform implements a decentralized consensus algorithm capable of meeting the performance requirements of applications on the blockchain called the Delegated Proof of Stake. Under this algorithm, a user who holds tokens on CyberWay blockchain, can select validators through a continuous approval voting system. Anyone can choose to participate in the block production and will be given an opportunity to produce blocks, provided this person can persuade token holders to vote for him/her.

More details about selecting validators can be found [here](https://docs.cyberway.io/validators/voting_for_validators).

## Bandwidth Resources
Account activity in the network is limited by the bandwidth allocated to it — the resources of bandwidth (CPU, NET, RAM and Storage). It is decreasing with a reduction of the bandwidth allocated. The bandwidth share is allocated to account in absolute accordance with funds on its balance. Bandwidth resources are located on the application balance and are allocated to acoount directly when this account performs a transaction in the system which ensures their dynamic distribution. User does not have to worry about in what proportion it is necessary to allocate funds for each of these bandwidth resources before performing transactions. The system automatically allocates the resources necessary for the transaction in accordance with their optimal consumption.

#### RAM
RAM, in CyberWay blockchain, is one of the important system resources consumed by blockchain accounts and smart contracts. RAM acts as a permanent storage and is used to store metadata, such as account names, permissions, token balance and other data for speedy on-chain data access.

#### Storage
Storage, in CyberWay blockchain, is referred to as `Storage bandwidth` and is used to store user's information (not metadata), such as posts, comments and so on. 

#### CPU
CPU, in CyberWay blockchain, represents the processing time of an action and is measured in microseconds (μs). CPU is referred to as `CPU bandwidth` in the `cleos` get account command output and indicates the amount of processing time an account has at its disposal when pushing actions to a contract.

#### Network (NET)
NET, in CyberWay blockchain, is the `network bandwidth`, measured in bytes, of transactions and is referred to as `NET bandwidth` on the `cleos` get account command.  

More details about the resources of bandwidth can be found [here](https://docs.cyberway.io/users/bandwidth_differences). 