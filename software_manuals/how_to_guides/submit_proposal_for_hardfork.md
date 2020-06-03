# How To Submit A Proposal For HardFork

### Goal
Submit a proposal to implement it via HardFork.  

### Before you begin
  * Install the currently supported version of `cleos`.
  * Understand the following:
    * Who is a worker.
    * What is a HardFork.
    * What is a [multisig propose transaction](https://docs.cyberway.io/software_manuals/command_reference/multisig#multisig-propose-transaction).
  

*Proposal* - an idea submitted by user to improve functionality or other characteristics of CyberWay blockchain. The proposal can be submitted as a separate post with a description of the idea without a way to implement it or with a description of the idea and a ready-made technical solution for its implementation.  

*Worker* - a user directly performing work in accordance with the statement of work. A user’s account can be an individual or a group of people.

### Steps
To submit a proposal you can use the following command:
```sh
$ cleos multisig approve <proposer> <proposal_name> <permissions>
```
  * `proposer`- user account submitting the proposal.
  * `proposal_name` - unique name assigned to the multi-signature transaction when it is created.
  * `permissions` - a level of permission required to approve the submitted proposal.

### Useful links
  * [Multisig approve action](https://docs.cyberway.io/devportal/system_contracts/cyber.multi-signature_contract#approve) .