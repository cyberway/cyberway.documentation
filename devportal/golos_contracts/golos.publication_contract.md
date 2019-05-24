# The Publication Smart Contract

## Overview

The `golos.publication` smart contract provides users to perform actions on posts, including: 
  * publishing posts; 
  * leaving comments to posts; 
  * voting for posts;
  * closing posts.
In addition, this contract contains logic for determining the payments to authors, curators and beneficiaries of posts.

## The list of actions implemented in the golos.publication smart contract
 
The `golos.publication` smart contract supports the following user actions: `setlimit`, `setrules`, `createmssg`, `updatemssg`, `deletemssg`, `downvote`, `upvote` and `unvote`.

## The setlimit action

The `setlimit` action is used to set rules that restrict user operations. The mechanism for restricting user operations is based on interaction of two smart contracts — precisely golos.publication and golos.charge. Each action of the `golos.publication` smart contract is linked to a certain battery of the `golos.charge` smart contract. In the `setlimit` parameters, it needs to specify an action (for example, `createmssg` or `upvote`) and a battery to be linked to it.  

The `setlimit` action has the following form:  
```cpp
setlimit(
    std::string act,
    symbol_code token_code,
    uint8_t    charge_id,
    int64_t    price,
    int64_t    cutoff,
    int64_t    vesting_price,
    int64_t    min_vesting
);
```
**Parameters:**  
  * `act` — a name of action.  
  * `token_code` —  token code (character string that uniquely identifies a token).  
  * `charge_id` — battery ID. The action specified as `act` is limited to the charge of this battery. Multiple actions can be linked to a single battery. For voting actions such as `upvote`, `downvote` and `unvote`, the battery ID value should be set to zero.    
  * `price` — a price (in arbitrary units) of the consumed battery recourse with the `charge_id` identifier for the `act` action. The battery recourse is reduced after each performed action and recovered with time.   
  * `cutoff` — lower threshold value of the battery recourse at which the `act` action is blocked.  
  * `vesting_price` — amount of vesting, that the user must pay for performing the `act` action, in case of exhaustion of the battery recourse (reaching lower threshold value). The `act` action will be executed if the user allows to withdraw the specified amount of vesting from her/his balance. For payment it is necessary that on the user balance there was a necessary sum of vesting in unblocked state.  
  * `min_vesting` — minimum value of vesting that a user needs to have on her/his balance to perform the `act` action.  

The interaction of smart publishing contracts and batteries allows a witness to flexibly configure restrictions on user actions (for example, such actions as voting for posts, publication of post and leaving comments can be correlated with the resources of three separate batteries. In this case, user activity will be limited for each of these actions. Also, all these actions can be linked to only one battery, that is, be limited by resources of the same battery). For each action performed by the user, she/he is charged a value corresponding to cost of the consumed battery recourse. When the `golos.charge` smart contract reaches the threshold value of used battery recourse, user's actions are blocked until necessary resource appears in the battery again.  

## The setrules action
The `setrules` action is used for setting rules that apply in application for distribution of rewards between authors and curators of posts.  
The `setrules` action has the following form:  
```cpp
void setrules(
    const funcparams&	  mainfunc,
    const funcparams&   curationfunc,
    const funcparams&   timepenalty,
    int64_t   curatorsprop,
    int64_t   maxtokenprop,
    symbol  tokensymbol
);
```  
**Parameters:**  
  * `mainfunc` — a function that calculates a total amount of rewards for an author and post curators in accordance with accepted algorithm (for example, a linear algorithm or a square root algorithm). The algorithm used in the function is selected by witnesses voting. The function contains two parameters: mathematical expression (the algorithm itself) by which the reward is calculated, and maximum allowable value of argument for this function. When setting parameter values for `setrules`, they are checked for correctness (including for monotonous behavior and for non-negative value).  
  * `curationfunc` — a function that calculates a fee for each of the curators in accordance with accepted algorithm (similar to calculation for `mainfunc`).  
  * `timepenalty` — a function that calculates a weight of vote, taking into account the time of voting and the penalty time duration.  
  * `curatorsprop` — total amount of fees for all curators.  
. * `maxtokenprop` — the maximum amount of tokens possible that can be assigned to author of post. This parameter is set by witnesses voting.  
  * `tokensymbol` — a token type (within the Golos application, only Golos tokens are used).  

To perform the `setrules` action, a user should have a witness authorization. In addition, the transaction must be signed by the `golos.publication` smart contract.  

## The createmssg action
The `createmssg` action is used to create a message as a response to a previously received (parent) message. The `createmssg` action has the following form:  
```cpp
void createmssg(
    name          account,
    std::string   permlink,
    name          parentacc,
    std::string   parentprmlnk,
    std::vector<structures::beneficiary> beneficiaries,
    int64_t        tokenprop,
    bool            vestpayment,
    std::string   headermssg,
    std::string   bodymssg,
    std::string   languagemssg,
    std::vector<structures::tag> tags,
    std::string   jsonmetadata
);
```  
**Parameters:**  
  * `account` — author name of the message.  
  * `permlink` — relative address of the message.  
  * `parentacc` — account name of the parent message author to which the response is formed via this `createmssg` action. In case of a zero value of `parentacc`, the created message will be considered as a post.  
  * `parentprmlnk` — relative address of the parent message.  
  * `beneficiaries` — a structure containing the names of beneficiaries and total amount of their fees. This amount is a percentage of total reward for the message.  
  * `tokenprop` — amount of tokens. This value cannot exceed the `maxtokenprop` value  specified in `set_rules`.  
  * `vestpayment` — `true`, if a user gives permission to pay in vestings in case of battery resource exhaustion (the message is sent regardless of battery resource). Default value is `false`.  
  * `headermssg` — title of the message.  
  * `bodymssg` — body of the message.  
  * `languagemssg` — language of the message.  
  * `tags` — tag that is assigned to the message.  
  * `jsonmetadata` — metadata in the JSON format.  

The pair of `parentacc` and `parentprmlnk` parameters identifies the parent message to which a response is created via `createmssg`.

To perform the `createmssg` action it is required that the transaction should be signed by the author of the message.

The key that is used to search for a message is bound to the `account` and `permlink` parameters.  

## The updatemssg action
The `updatemsg` action is used to update a message previously sent by user.  
The `updatemssg` action has the following form:  
```cpp
void updatemssg(
    name           account,
    std::string    permlink,
    std::string    headermssg,
    std::string    bodymssg, 
    std::string    languagemssg,
    std::vector<structures::tag> tags,
    std::string    jsonmetadata
);
```
**Parameters:**  
  * `account` — author name of the message being updated.
  * `permlink` — relative address of the message.  
  * `headermssg` — title of the message.
  * `bodymssg` — body of the message.  
  * `languagemssg` — language of the message.  
  * `tags` — tag that is assigned to the message.  
  * `jsonmetadata` — metadata in the JSON format.  

To perform the `updatemssg` action it is required that the transaction should be signed by the author of the message.

## The deletemssg action
The `deletemssg` action is used to delete a message previously sent by user.  
The `deletemssg` action has the following form:  
```cpp
void deletemssg(
    name account,
    std::string permlink
);
```
**Parameters:**  
  * `account` — author name of the message being deleted.  
  * `permlink` — relative address of the message being deleted.  

 
The message cannot be deleted in the following cases:  
  * the message has a comment;  
  * total weight of all votes cast for this message is greater than zero. The weight of the user's vote depends on amount of vesting she/he uses. A message cannot be deleted if the total weight of all votes has a positive value.  

To perform the `deletemssg` action it is required that the transaction should be signed by the author of the message.

## The upvote action
The `upvote` action is used to cast a vote in the «upvote» form when voting for a message.  
The `upvote` action has the following form:  
```cpp
void upvote(
    name voter,
    name author,
    std::string permlink,
    uint16_t weight
);
```
**Parameters:**  
  * `voter` — voting account name.  
  * `author` — author name of the message being voted for.  
  * `permlink` — relative address of the message for which the `voter` account is voting.  
  * `weight` — the vote weight of the account name `voter`.  

The key that is used to search for a message is bound to the `account` and `permlink` parameters.  

To perform the `upvote` action it is required that the transaction should be signed by the account name `voter`. 

## The downvote action
The `downvote` action is used to cast a vote in the «downvote» form when voting for a message.  
The `downvote` action has the following form:  
```cpp
void downvote(
    name voter,
    name author,
    std::string permlink,
    uint16_t weight
);
```
**Parameters:**  
  * `voter` — voting account name.  
  * `author` — author name of the message being voted for.  
  * `permlink` — relative address of the message for which the `voter` account is voting.   
  * `weight` — the vote weight of the account name `voter`.  

The key that is used to search for a message is bound to the `author` and `permlink` parameters.    

To perform the `downvote` action it is required that the transaction should be signed by the account name `voter`.  

## The unvote action
The `unvote` action is used to revoke user's own vote that was previously cast for the message.  
The `unvote` action has the following form:  
```cpp
void unvote(
    name voter,
    name author,
    std::string permlink
);
```

**Parameters:**  
  * `voter` — account name that revokes her/his own vote previously cast for the message.  
  * `author` — author name of the message from which the vote is being removed.  
  * `permlink` — relative address of the message from which the vote is being removed.  

The key that is used to search for a message is bound to the `author` and `permlink` parameters.  
 
To perform the `unvote` action it is required that the transaction should be signed by the account name `voter`.

## Other parameters which are used and set in the golos.publication smart contract
 There are other parameters in the `golos.publication` smart contract that can be set by calling `set_params`:  
  * `cashout_window` — time interval after which payment of rewards for a publication is possible.  
  * `max_vote_changes` — maximum possible number of user's re-votes (it shows how many times a user can change her/his vote for the same post).  
  * `max_beneficiaries` — maximum possible number of beneficiaries.
  * `max_comment_depth` — maximum allowable nesting level of comments (it shows the allowed nesting level of child comments relative to parent one).  
  * `social_acc` — account name of the `golos.social` smart contract.  
  * `referral_acc` — account name of the `golos.referral` smart contract.  
  
****
