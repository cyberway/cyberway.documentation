
# Section_1 General

## Purpose
CyberWay blockchain platform is developed on the basis of EOS logic to provide service to users of different communities. The main direction of CyberWay development is the creation of a decentralized system with the support of several applications operating on the basis of smart contracts on one blockchain simultaneously.  

The implementation of CyberWay in the test version (hereinafter - Testnet) is intended for testing in terms of installing and running CyberWay on the server, as well as for conducting debugging work on the results of the tests.

## Installation and Testnet Test
Instructions for installing and testing Testnet are conditionally divided into the following steps:
  * settings;
  * setting up services;
  * construction of containers;
  * building a Docker image.


Testnet is installed on the server using the available Docker platform and the Docker-compose command line tool. Using the Docker platform provides:
  * creating the necessary software environment, including the required list of libraries, regardless of the version of the operating system;
  * creation of an environment isolated from unnecessary temporary files stored by the system in the newly constructed space.

To set the `nodeos` configuration, the `config.ini` file is used.
To build the Docker container, use the Docker file. To configure a set of services, use `docker-compose.yml` (Testnet requires setting up two services — `nodeosd` and `mongo`).  

The server on which Testnet is installed should have the following characteristics:
  * объем памяти RAM (не менее):  8 ГБ;
  * объем дисковой памяти (не менее): 20 ГБ.

The following software must also be installed on the server:
  * operating system:
    * Ubuntu (version recommended: 16.04 or 18.04); 
    * MacOS Darwin 10.12 (or later versions);
    * Centos 7;
    * Fedora 25 (or later version);
    * Mint 18;
  * nodeos utility version 15.0 (or later version); 
  * cleos utility version 15.0 (or later version);
  * keosd utility version 15.0 (or later version);
  * graphene library;
  * docker; 
  * docker compose;
  * compier: eosio-cpp.

If there is no server with the required operating system, use the server with the operating system of the Linux family. It is possible to install Testnet on such a server using the Docker platform, which ensures the creation of the necessary environment, regardless of the version of the Linux system.
The installation and operation of Testnet on a server running any other system classes is not supported.  

To install Testnet on a server using the Docker platform, you must perform the following operations:
  * configure the Docker image in a separate space;
  * create containers using a docker image. Containers can be placed both on local, and on remote or virtual computer.

To test the functioning of Testnet, you need to connect to the node via `cleos`.


## Genesis Data
The genesis information about the blockchain and the blockchain data Golos recorded on a specific block are used as the initial data when installing Testnet on the server. By now, the following objects have been moved and are available for installing Testnet on the server:
  * user accounts. For each account in the Golos blockchain, a CyberWay account is created and linked to the username in the @golos domain. For example, a user registered in the Golos blockchain as <username> will be named in the Testnet as <username>@golos. This name will be used in transactions;
  * public keys. For each account of the blockchain Golos, its public keys were transferred with the corresponding privileges, including `owner`, `active` and `posting`.

## Recommendation
> Before starting the instructions given in the manual, it is highly recommended **to save the private key code**.
