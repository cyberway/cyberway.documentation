
# 1 Preparatory work  

## Stages of building and deploying an application on CyberWay    
  * Preliminary organizational work.  
  * Software development:  
    * development of contracts that implement the logic of the application algorithms;  
    * deployment of contracts on the blockchain node;  
    * contract testing.  

## Hardware and software requirements
It is recommended to build and debug contracts on a specific server where Testnet is installed. The server must meet the following characteristics (or above):
  * RAM size (a min. of): 8 GB;
  * disk space (a min. of): 20 GB.

The following software must also be installed on the server:
  * an operating system:
    * Ubuntu (version recommended: 16.04 or 18.04); 
    * MacOS Darwin 10.12 (or later versions);
    * Centos 7;
    * Fedora 25 (or later versions);
    * Mint 18;
  * a Nodeos utility version 15.0 (or later versions);
  * a cleos utility version 15.0 (or later versions);
  * a keosd utility version 15.0 (or later versions);
  * a graphene library;
  * a docker;
  * a docker compose;
  * a compiler: eosio-cpp;
  * an ABI-generator: eosio-abigen.

Upon development and debugging the contracts can be loaded into the MainNet.  

**Please note:**  
> The required disk space for node running Mainnet will be determined later.  


## Knowledge needed to develop applications for CyberWay
  * Some basic knowledge of blockchain technology;
  * An ability to create an account;
  * An ability to operate a wallet;
  * An ability to compile different programs written in C++.

## Preliminary organizational work
Before development it is necessary to determine the principles (rules) of the application. CyberWay provides resources and service software components which allow you to create all the contracts necessary for the implementation of application logic of different complexity.  

The application contracts can be developed both from scratch as well as using previously developed contracts (for example, based on the contracts of [Golos application](https://cyberway.gitbook.io/en/devportal/golos_contracts)).  

However, first, it is advisable to do the following:  
  * Identify the tasks that the new application will solve. Outline the rules within which these tasks could be solved.  

  * Develop an entire methodology for technical support of the application, including ways to involve technical experts in the development of the applications.  

  * Build thoroughly structural (for instance, block) schemes of the algorithms for successful performance of the application according to its rules. Determine a set of contracts that is required for implementation of the algorithms.  

  * Prepare a server (blockchain node) that will be linked to your application. Ensure that the Testnet software is already installed on the server and the developers has a wallet as well as private and public keys. Otherwise, it is necessary to:  
    * Install the latest testnet version on the server, following the instructions of the [Testnet Installation Guide](https://cyberway.gitbook.io/ru/v/ru/producers/testnet_installation);  
    * Create a user wallet following our [guidelines for creating a wallet](https://cyberway.gitbook.io/ru/v/ru/developers/create_development_wallet).  

****  
