# How to launch EOS dApps on CyberWay

CyberWay services allow users to create their own applications with their own tokens, as well as migrate applications from other platforms and deploy them on the CyberWay blockchain while preserving accounts, keys, and funds. Thus in August 2019, [transit](https://docs.cyberway.io/validators/transit_guide) of the Golos blockchain to the CyberWay platform was successfully completed.  

EOS dApp developers can easily duplicate their dApp onto CyberWay with just a few clicks, and gain an immediate access to CYBER Token holders. You are welcome to follow the instructions provided below.  

**Step_1** Visit a CyberWay’s dApp to create an account (i. e. [commun.com](https://commun.com)).  
 
**Step_2** Make sure to have enough CYBER tokens staked in your account to allocate resources. To convert tokens into CYBER you can use the following command:
```
cleos transfer <your account> cyber.stake <quantity>
```

Visit [Stake Usage Guide](https://docs.cyberway.io/validators/stake_usage_guide) for more information.  

**Step_3** Install the standard cleos developer tool.  
If you want to deploy the dApp on a separate server, we recommend that you read this [guide](https://docs.cyberway.io/devportal/create_application) first.  

**Step_4** Ensure that you have compiled your contract with a compatible CDT (Contract Development Toolkit). Currently this is [CyberWay CDT](https://doxygen-cdt.cyberway.io/index.html) based on EOSIO CDT 1.6.3 .  

**Step_5** Create an account for your smart contract:
```
cleos  system newaccount <Creator> <contractAccount> <contractAccountOwnerKey> <contractAccountActiveKey> --stake “100.0000 CYBER”
```

**Step_6** Deploy your smart contract:
```
cleos set contract <contractAccount> <compiled contract code directory>  --abi <abi file name.abi> -p <contractAccount>@active
```

**Step_7** Make sure the process is completed successfully.  
```
cleos get info
```

It is considered, dApp has successfully been deployed if no errors appeared at run time.

**Useful links**  
[How to register as a validator candidate](https://docs.cyberway.io/validators/quick_reference)  
[Wallet and Key for Development](https://docs.cyberway.io/devportal/create_development_wallet)  
[Mainnet Connection Guide](https://docs.cyberway.io/validators/mainnet_connection)  
[Referral program](https://docs.cyberway.io/devportal/application_contracts/golos_contracts/golos.referral_contract)  


*****  
