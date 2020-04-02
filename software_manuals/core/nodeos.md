# Nodeos

### Introduction
`nodeos` is the core service daemon that runs on every CyberWay node. It can be configured to process smart contracts, validate transactions, produce blocks containing valid transactions, and confirm blocks to record them on the blockchain.

> **Access Node**
> A local or remote CyberWay access node running `nodeos` is required for a client application or smart contract to interact with the blockchain.


## Nodeos Usage

`nodeos` is a command line interface (CLI) application. It can be started manually from the command line or through an automated script. Nodeos options are used mainly for housekeeping purposes, such as setting the directory where the blockchain data resides, specifying the name of the `nodeos` configuraton file, setting the name and path of the logging configuration file, etc. All CLI options can be found by running `nodeos --help` as shown below.
```sh
$ nodeos --help
```

```
Application Options:
Application Config Options:
  --plugin arg                          Plugin(s) to enable, may be specified 
                                        multiple times

Application Command Line Options:
  -h [ --help ]                         Print this help message and exit.
  -v [ --version ]                      Print version information.
  --print-default-config                Print default configuration template
  -d [ --data-dir ] arg                 Directory containing program runtime 
                                        data
  --config-dir arg                      Directory containing configuration 
                                        files such as config.ini
  -c [ --config ] arg (=config.ini)     Configuration file name relative to 
                                        config-dir
  -l [ --logconf ] arg (=logging.json)  Logging configuration file name/path 
                                        for library users
```