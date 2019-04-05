
# 2 Cоздание простого контракта
Многие действия по созданию контракта являются типовыми. Отличия состоят только в реализации выполняемых контрактом функций, которые реализуются непосредственно в теле контракта. В этом разделе в качестве примера приведена инструкция по созданию контракта, функцией которого является выдача приветствия в виде «Hello, user».  

**2.1 Создать директорию для развертывание в ней контрактов**  
В созданную директорию с именем CONTRACTS_DIR загрузить компоненты CDT (англ. Contract Development Toolkit), необходимые для компилирования контрактов.
```
cd CONTRACTS_DIR
git clone --recursive https://github.com/GolosChain/cyberway.cdt --branch <branch name> --single-branch
cd cyberway.cdt
./build.sh
sudo ./install.sh
```  
**2.2 Создать файл hello.cpp**
```
cd CONTRACTS_DIR
mkdir hello
cd hello
touch hello.cpp
```  
Заполнить файл  hello.cpp следующим текстом, в котором реализовано действие (action) для отправки сообщения в виде «Hello, user».
```cpp
#include <eosiolib/eosio.hpp>

using namespace eosio;

class [[eosio::contract("hello")]] hello : public contract {
  public:
      using contract::contract;

      [[eosio::action]]
      void hi( name user ) {
         print( "Hello, ", user);
      }
};

EOSIO_DISPATCH( hello, (hi))
```  
Данное действие принимает параметр с именем «user» и в качестве результата выводит на печать сообщение вида «Hello, user».
EOSIO_DISPATCH — макрос для обработки данного действия.  

**2.2 Скомпилировать файл hello.cpp**
```
eosio-cpp -o hello.wasm hello.cpp --abigen
```  

**2.3 Установить (развернуть) контракт**  
Во время установки контракта создается аккаунт этого контракта, а также публичный ключ (public key) данного аккаунта.  
```
cleos wallet keys
cleos create account cyber hello <public key> -p cyber@active
```   
Руководство по созданию кошелька (wallet), а также по созданию ключа для разработки можно найти на [сайте](https://cyberway.gitbook.io/ru/v/ru/developers/create_development_wallet) CyberWay.  

**2.4 Установить абсолютный путь к созданному контракту**  
Указать абсолютный путь `<contracts dir path>` к каталогу контрактов в следующей команде:
```
cleos set contract hello CONTRACTS_DIR/hello -p hello@active
```  
**2.5 Проверить функционирование контракта**
Для проверки функционирования контракта можно вызвать действие с указанием имени пользователя, например, послать приветствие пользователю «Bob», исполнив: 
```
cleos push action hello hi '["bob"]' -p hello@active
```  

Функционирование контракта будет считаться успешным, если на монитор будет выдана информация вида:
```
executed transaction: ... 
#    hello.code <= hello.code::hi               {"user":"bob"}
>> Hello, bob
```  

Для расширения функций контракта необходимо расширить логику файла hello.cpp. Именно от логики файла file_name.cpp зависят функциональные возможности создаваемого контракта. 
