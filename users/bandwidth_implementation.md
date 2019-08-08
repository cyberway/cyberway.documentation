With this technical post we’re determined to record all the key points related to the technical implementation of the bandwidth in writing, which were covered by the Golos • Core team during the october Discord meeting itself (04.10.18 to be precise); familiarize both GOLOS users and developers with all the subtleties and reinforce the basic understanding with the help of some examples.  


## Bandwidth implementation within the old ChainBase platform
In the application of GOLOS, the user's activity within the network happens to be limited to the bandwidth allocated to him.  

The bandwidth value for each user is calculated on an individual basis in direct proportion to Golos Power (GP) available in his own balance. The initial balance amount is provided by vesting crediting courtesy the parent application account through which the registration actually takes place, in our case, for instance, it is operated through Golos.io.  

The user can maintain activity in the system (for example, operate on transactions for publications or voting) only if there is a vesting sum equal to at least a base value within his own balance.  

The minimum value of this base amount of funds in GOLOS is determined by delegates voting. To actually determine the base value, it is necessary to take into account the current rate of the Golos Power to GOLOS and the minimum bandwidth necessary to perform all the basic operations. For example, there is a bandwidth dependence on the size of the post published by the user and his current activity in the network (the average size of the blocks generated). Another example is the minimum Golos Power when voting for posts.  

In the latest releases of the platform, it became possible to delegate the Golos Power with tangible detail added: instead of transferring the assets to the balance of a new user they simply become available for temporary use, which makes a crucial change to the economics of the application and makes it possible to spend funds from the balance of the application account more effectively.  

During the use of both mechanisms, the application’s account will need to identify a middle ground for the value of funds transferred or delegated, in which the Golos Power on the balance of the new account will be sufficient for the normal activity of the active user, but most of the funds will not be blocked for inactive users.  

## Bandwidth sharing
In the GOLOS application based on the CyberWay platform, which is a fork of the EOS system, the algorithm for calculating the bandwidth for the account is different as it does not depend on the Golos Power value anymore. Indeed, the bandwidth value is calculated in the separate order for CPU, NET, RAM and Storage and is paid by the blockchain system tokens rather than the application tokens.  

A distributed application can have a total bandwidth on its balance sheet. Due to the fact that the behavior after registration from user to user may vary, the task of systematizing the use of the common bandwidth was solved, and the efficiency of its use was increased.  

Thus, a dynamic distribution of resources of the total bandwidth is assumed depending on the actual use by a particular user. In other words, it was decided to allocate the user’s bandwidth share for the application as needed, that is, when the user performs transactions in the system.  

This task can be accomplished using the implementation of two-step subscription bandwidth usage. There is a smart contract managing the multisig-account application in the prototype created. The most active members of the community among the elected ones are included in the number of the signatories of the account from the side of the application - obviously, the list of signatories to this account may vary.  

To put a signature on behalf of the multisig-account of the application in order to use the resources of the common bandwidth, the community generates a pair of private-public keys. The public key is stored securely in the blockchain, while the private community key is stored in a secure part of the service (normally, a website). The signature on the use of resources is stamped with a private key, and the check is performed with a public key. Signers of a multisig-account can at any time change the common private key to access the bandwidth in case of inappropriate use. The private key allows you to use bandwidth, but does not allow it to be liquidated or sold (for example, on any exchange).  

It is worth mentioning that the multisig-account of the application has the rights to buy and sell the bandwidth, provided that the majority of delegates sign their names under this operation. At the same time, the generated key can only be used to allocate bandwidth, and for protection purposes, its regular regeneration is recommended.  

When creating a new user account, it becomes necessary to pay for the storage location of the account in the blockchain's RAM. So that the user would pass the payment for the creation of a new account and the application would not spend additional money for that too, there is a possibility for the new account to remain on the community balance. In this case, the account is created with two pairs of public-private keys. The account has a threshold equal to 2 (two private signatures are required to complete the transaction), and each key has a weight equal to 1. One private key is given to the user, and the second is stored in the protected part of the community service.  

As a result, each transaction created by a user is signed by two keys - a user private key and a private key belonging to the community. A transaction can go into a blockchain only if there are signatures of these two keys, that is, the user cannot perform actions without the participation of the application, and the application cannot perform actions without the participation of the user.  

The implementation of this solution allows you to more flexibly allocate bandwidth resources. For example, in the event that a user with low activity appears in the system, limited to registration, a minimal amount of resources will be allocated for its use. In the future, community delegates will be able to adjust the bandwidth (buy it for new users in case of lack of resources, etc.).  

It is important that each user at any time has the opportunity to redeem his account from the application and exit the two-sign rule for each transaction, which requires redeeming space for storing his account from the community for system tokens. The redemption cost is determined by the blockchain rules - this is the number of system tokens that must be paid for RAM to store the account with its balances. The user needs to take into account that in order to complete transactions, he will need to purchase blockchain system tokens for using the bandwidth resources, such as CPU and NET.  

In order to summarize, CyberWay platform will supposedly represent two types of users: those using the personal bandwidth and those using the total bandwidth available on the balance of a certain application.  

It is important to add that this system has the right to exist not only within the framework of the “application + user” scenario, but also into the “user + another user” scheme. Any account will be able to sell its own bandwidth at fixed prices, which will form an alternative liquid bandwidth scheme in the system.  

*****  
