
# 3 Создание токенов

В CyberWay каждый пользователь приложения может создать собственный вид токена (в EOS создавать токены могут только валидаторы, что дает валидаторам власть над приложениями). Токены создаваемого приложения должны разворачиваться на отдельном от `cyber.token` аккаунте. Пользователь при создании токенов может использовать эталонную реализацию токен-контракта `cyber.token` в качестве базовой, загрузив его содержимое в свою область. То есть пользователю необходимо создать свой аккаунт и свой вид токена.  

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
cleos create account olga olga.token <публичный  ключ>
```  
Параметры командной строки:  
olga — имя аккаунта для создаваемого контракта;  
olga.token — имя токен-контракта, загруженного с репозитория `cyberway.contracts` с исходными файлами.  

В качестве примера создан аккаунт контракта с именем `olga.token`.  

**3.3 Скомпилировать контракт**  
```
eosio-cpp -I include -o olga.token.wasm src/olga.token.cpp --abigen
```
Контракт компилируется в веб-ассемблеровский файл формата `wasm`. Наличие опции `--abigen` указывает, что будет также сгенерирован файл `abi/olga.token.abi`.   

**3.4 Установить токен-контракт**
```
cleos set contract olga.token CONTRACTS_DIR/cyberway.contracts/olga.token --abi abi/olga.token.abi -p olga.token@active
```  
Параметр:  
`olga.token@active` — имя, которое будет использоваться для авторизации запроса.  

Токен-контракт будет считаться успешно установленным, если в результирующей выдаче выполняемой команды будет содержаться информация вида:
```.
executed transaction:  ... 
#         eosio <= eosio::setcode               {"account":"olga.token","vmtype":0,"vmversion":0,"code":"<code>
#         eosio <= eosio::setabi                {"account":"olga.token","abi":"<code>
warning: transaction executed locally, but may not be confirmed by the network yet         ]
```  
 
**3.5 Создать токен**  
Для создания нового токена используется операция-действие `create`. В качестве аргумента задается тип токена `symbol_name`, содержащий два значения  — максимальное значения предложения и символ токена. Вызов данного действия имеет вид:
```
cleos push action olga.token create '{"issuer":"olga", "maximum_supply":"1000000000.0000 SYS"}' -p olga.token@active
```  
Наличие опции `-p olga.token@active` санкционирует контрактом `olga.token` выполнение этого действия.  
  
Создание токена считается успешным, если на мониторе появится информация вида:  
```
executed transaction: <info>
#   olga.token <= olga.token::create          {"issuer":"olga","maximum_supply":"1000000000.0000 SYS"}
```
В результате будет создан новый токен SYS,имеющий точность четыре десятичных знака. Максимально допустимое количество токенов в обращении должно быть ограничено значением 1000000000. Для создания этого токена требуется разрешение контракта `olga.token`. Имя `olga.token@active` будет использоваться для авторизации запроса.  

**3.6 Выпуск токенов**  
Автор токена может выпускать токены на предварительно созданный аккаунт, например, на аккаунт с именем «alice», исполнив:
```
cleos push action olga.token issue '[ "alice", "100.0000 SYS", "memo" ]' -p olga@active

```
В результате выполнения команды должна появиться информация вида:
```
executed transaction:  ... 
#   olga.token <= olga.token::issue           {"to":"user","quantity":"100.0000 SYS","memo":"memo"}
>> issue
#   olga.token <= olga.token::transfer        {"from":"olga","to":"alice","quantity":"100.0000 SYS","memo":"memo"}
>> transfer
#         olga<= olga.token::transfer        {"from":"olga","to":"alice","quantity":"100.0000 SYS","memo":"memo"}
#          user <= olga.token::transfer        {"from":"olga","to":"alice","quantity":"100.0000 SYS","memo":"memo"}
```  

Выдача содержит одно действие `issue` и три действия `transfer`. Во время выполнения `issue` дополнительно генерируются три внутренних вызова с уведомлением отправителя и получателя токенов.  

**3.7 Трансфер токенов**  
Часть токенов может быть переведена с баланса одного аккаунта на баланс другого аккаунта. Например, для перевода суммы 25 токенов с баланса аккаунта «alice» на баланс аккаунта «bob» необходимо использовать командную строку вида:
```
cleos push action olga.token transfer '[ "alice", "bob", "25.0000 SYS", "m" ]' -p alice@active
```  
Для выполнения этого действия требуется разрешение аккаунта отправителя «alice» — наличие опции `-p alice@active`.  

Трансфер токенов считается выполненным успешно, если по завершению действия в командном окне появляется информация вида:
```
executed transaction:  ... 
#   olga.token <= olga.token::transfer        {"from":"alice","to":"bob","quantity":"25.0000 SYS","memo":"Here you go bob!"}
>> transfer
#          user <= olga.token::transfer        {"from":"alice","to":"bob","quantity":"25.0000 SYS","memo":"Here you go bob!"}
#        tester <= olga.token::transfer        {"from":"alice","to":"bob","quantity":"25.0000 SYS","memo":"Here you go bob!"}
```  
Для контроля тансвера токенов можно воспользоваться вызовом  `get_currency_balance` для получения данных баланса аккаунтов отправителя и получателя, исполнив:
```
cleos get currency balance olga.token alice SYS
cleos get currency balance olga.token bob SYS
```