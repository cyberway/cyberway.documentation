
# APPENDIX B

## Procedure for checking the creation of blocks

**Monitoring the creation of blocks by the node**  

Make sure that the node has started to produce blocks in CyberWay Mainnet. The log file should contain a piece of text like this one:
```
info  2019-10-01T01:05:33.011 nodeos    producer_plugin.cpp:1616      produce_block        ] Produced block 0e28d615daa07d9b... #1215523 @ 2019-10-01T01:07:00.000 signed by <validator name> [trxs: 0, lib: 1215441, confirmed: 15]
```
The `produce_block` field indicates the block created by the node and signed by <validator name>. Check if this name is your.

**Note**  
*It should be noted that the node will produce blocks only if the active schedule contains an account of this validator. To verify if your name in the active schedule list, use the command*
```  
sudo cleos-seed get schedule  
``` 

Also, make sure that the information about blocks received over the network is regularly saved to the log file. It can be checked with `cleos get info`. 


## Conclusion
The node is considered to be successfully produced blocks, if the log file contains information similar to the one in this appendix.
