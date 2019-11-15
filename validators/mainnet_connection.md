# Mainnet Connection Guide

This Guide is meant to help the CyberWay validator, as well as the personnel having a server at their disposal, in installing CyberWay software on the server and connecting it to Mainnet.  

The Guide contains description of two variants for connecting a server to Mainnet — connecting the server as a seed-node and connecting it as a validator node. It is assumed that the server is running under Ubuntu 16.04 operating system or another Linux family system using Docker and Docker-compose software.  

The instructions given in this guide concern the personnel with basic computer skills and familiar with the basic concepts of blockchain technology.  

**This Guide provides:**  
  * downloading the CyberWay software to the server;
  * configuring `nodeos` service;
  * startup of the Docker container;
  * setting up the `nodeosd` and `mongo`;
  * checking server readiness to connect to Mainnet.

