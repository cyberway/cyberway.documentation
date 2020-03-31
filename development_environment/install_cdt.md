# 2: Install the Contract Development Toolkit

This section provides guidance on how to install `cyberway.cdt` tools on your server.  

The CyberWay Contract Development Toolkit (CyberWay CDT) is based on EOSIO CDT and is a collection of tools related to contract compilation. CyberWay CDT is primarily used for compiling contracts and generating ABI.  

It is recommended to build and debug contracts on a specific server. Therefore, the server where CDT is installed, must meet the following characteristics (or above):
  - RAM size (a min. of): 8 GB
  - disk space (a min. of): 20 GB

One of the following operating systems must also be installed on the server:
  - Ubuntu 16.04
  - Ubuntu 18.04
  - MacOS Darwin 10.12 (or later versions)
  - Centos 7
  - Fedora 25 (or later versions)
  - Mint 18


**Attention**  
  - If you have previously installed CyberWay.CDT, run the `uninstall.sh` script (it is in the root of CyberWay.CDT repository) before downloading and using the binary releases.

## Install cyberway.cdt on local server

Installing `cyberway.cdt` requires you to perform the following actions:  
1. Cloning the `cyberway.cdt` repository to your server  
2. Building binaries  
3. Tools installation  

The last two actions are performed by scripts `build.sh` and `install.sh` located in the root of `cyberway.cdt`. These scripts are universal and designed for all operating systems supported by the CyberWay platform.

#### Clone the cyberway.cdt repository

The location where `cyberway.cdt` is cloned is not that important because `cyberway.cdt` will be installing as a local binary in later steps. You can clone `cyberway.cdt` to "contracts" directory previously created or to any other location on your local system that is fit.

```sh
 $ cd CONTRACTS_DIR
 $ git clone --recursive https://github.com/cyberway/cyberway.cdt
```

#### Build binaries
```sh
 $ cd cyberway.cdt
 $ ./build.sh
```

#### Install tools
```sh
 $ sudo ./install.sh
```

The following tools will be installed to your local machine: 
  - cyberway-abidiff
  - cyberway-cpp
  - eosio-abigen
  - eosio-cc
  - eosio-init
  - eosio-ld
  - eosio-objcopy
  - eosio-pp
  - eosio-wasm2wast
  - eosio-wast2wasm
  - llvm-ar
  - llvm-nm
  - llvm-objdump
  - llvm-ranlib
  - llvm-readelf
  - llvm-readobj
  - llvm-strip


#### Uninstall
```sh
 $ cd cyberway.cdt
 $ sudo ./uninstall.sh
```

The `install.sh` and `uninstall.sh` scripts need to be ran with `sudo` because various binaries of `cyberway.cdt` will be installed locally. It needs to be typed computer's account password.

## Important
Installing `cyberway.cdt` will make the compiled binary global, therefore it can be accessable anywhere. For this tutorial, it is strongly suggested that you do not skip the install step for `cyberway.cdt`, failing to install will make it more difficult to follow this and other tutorials and make usage more difficult in general.

