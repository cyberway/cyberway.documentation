
# 3 Создание токенов
**3.1 Загрузить исходные файлы контрактов**  
Войти в созданную для контрактов директорию и загрузить в нее копию удаленного репозитория с исходными файлами контрактов.
```
cd CONTRACTS_DIR
git clone https://github.com/GolosChain/cyberway.contracts --branch <branch name> --single-branch
```
Репозиторий cyberway.contracts содержит несколько контрактов, но для создания токенов необходим контракт `cyber.token`. 

```
cd cyberway.contracts/cyber.token
```  

**3.2 Создать аккаунт для контракта**  
Перед тем, как развернуть токен-контракт, необходимо создать аккаунт этого контракта (учетную запись контракта), исполнив командную строку вида:  
```
cleos create account account_name cyber.token <буквенно-цифровая запись ключа>
```  
Параметры командной строки:  
account_name — имя аккаунта для создаваемого контракта;
cyber.token — имя токен-контракта, загруженного с репозитория `cyberway.contracts` с исходными файлами.  

**3.3 Скомпилировать контракт**  
```
eosio-cpp -I include -o cyber.token.wasm src/cyber.token.cpp --abigen
```
Контракт компилируется в веб-ассемблеровский файл формата `wasm`. Наличие опции `--abigen` указывает, что будет также сгенерирован файл `abi/cyber.token.abi`.  

**3.4 Установить токен-контракт**
```
cleos set contract cyber.token CONTRACTS_DIR/cyberway.contracts/cyber.token --abi abi/cyber.token.abi -p cyber.token@active
```  
Параметр:  
`cyber.token@active` — имя, которое будет использоваться для авторизации запроса.  

Токен-контракт будет считаться успешно установленным, если в результирующей выдаче выполняемой команды будет содержаться информация вида:
```.
executed transaction:  ... 
#         eosio <= eosio::setcode               {"account":"eosio.token","vmtype":0,"vmversion":0,"code":"<code>
#         eosio <= eosio::setabi                {"account":"cyber.token","abi":"<code>
warning: transaction executed locally, but may not be confirmed by the network yet         ]
```   
**3.5 Создать токен**  
Для создания нового токена используется операция-действие `create`. В качестве аргумента задается тип токена `symbol_name`, содержащий два значения  — максимальное значения предложения и символ токена. Вызов данного действия имеет вид:
```
cleos push action cyber.token create '{"issuer":"cyber", "maximum_supply":"1000000000.0000 SYS"}' -p cyber.token@active
```  
Наличие опции `-p cyber.token@active` санкционирует контрактом `cyber.token` выполнение этого действия.  
  
Создание токена считается успешным, если на мониторе появится информация вида:  
```
executed transaction: <info>
#   cyber.token <= cyber.token::create          {"issuer":"cyber","maximum_supply":"1000000000.0000 SYS"}
```
В результате будет создан новый токен SYS,имеющий точность четыре десятичных знака. Максимально допустимое количество токенов в обращении должно быть ограничено значением 1000000000. Для создания этого токена требуется разрешение контракта `cyber.token`. Имя `cyber.token@active` будет использоваться для авторизации запроса.  

**3.6 Выпуск токенов**  
Автор токена может выпускать токены на предварительно созданный аккаунт, например, на аккаунт с именем «alice», исполнив:
```
cleos push action cyber.token issue '[ "alice", "100.0000 SYS", "memo" ]' -p cyber@active

```
В результате выполнения команды должна появиться информация вида:
```
executed transaction:  ... 
#   cyber.token <= cyber.token::issue           {"to":"user","quantity":"100.0000 SYS","memo":"memo"}
>> issue
#   cyber.token <= cyber.token::transfer        {"from":"cyber","to":"user","quantity":"100.0000 SYS","memo":"memo"}
>> transfer
#         cyber <= cyber.token::transfer        {"from":"cyber","to":"user","quantity":"100.0000 SYS","memo":"memo"}
#          user <= cyber.token::transfer        {"from":"cyber","to":"user","quantity":"100.0000 SYS","memo":"memo"}
```  

Выдача содержит одно действие `issue` и три действия `transfer`. Во время выполнения `issue` дополнительно были сгенерированы три внутренних вызова с уведомлением отправителя и получателя токенов.  

**3.6 Трансфер токенов**  
Часть токенов может быть переведена с баланса одного аккаунта на баланс другого аккаунта. Например, для перевода суммы 25 токенов с баланса аккаунта «alice» на баланс аккаунта «bob» необходимо использовать командную строку вида:
```
cleos push action cyber.token transfer '[ "alice", "bob", "25.0000 SYS", "m" ]' -p alice@active
```  
Для выполнения этого действия требуется разрешение аккаунта отправителя «alice» — наличие опции `-p alice@active`.  

Тансфер токенов считается выполненным успешно, если По завершении действия в командном окне должна появится информация вида:
```
executed transaction:  ... 
#   cyber.token <= cyber.token::transfer        {"from":"alice","to":"bob","quantity":"25.0000 SYS","memo":"Here you go bob!"}
>> transfer
#          user <= cyber.token::transfer        {"from":"alice","to":"bob","quantity":"25.0000 SYS","memo":"Here you go bob!"}
#        tester <= cyber.token::transfer        {"from":"alice","to":"bob","quantity":"25.0000 SYS","memo":"Here you go bob!"}
```  
Для контроля тансфера токенов можно воспользоваться вызовом  `get_currency_balance` для получения данных баланса аккаунтов отправителя и получателя, исполнив:
```
cleos get currency balance cyber.token alice SYS
cleos get currency balance cyber.token bob SYS
```