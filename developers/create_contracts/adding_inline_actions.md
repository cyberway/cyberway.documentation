
# 7 Создание встроенных вызовов для действий

В этом разделе приведены инструкции по созданию в контракте встроенных вызовов (inline)  для действий - операций вида `action`. Операция (action) может быть вызвана другой операцией этого же контракта. Примеры выполнения данных инструкций показаны на контракте адресной книги `addressbook`, создание которого изложено в [разделе 5](https://cyberway.gitbook.io/ru/v/master-ru/developers/create_contracts/data_persistence) настоящего руководства.  
[ссылка 1 на 5](./data_persistence)  
[ссылка 2 на 5](data_persistence)  
[ссылка 3 на 5](../data_persistence)  
[ссылка 4 на 5](/data_persistence.md)  
[ссылка 5 на 5](data_persistence.md)  
  

**7.1 Добавить cyber.code для разрешения на действие**   
Чтобы действие, находящееся в контракте адресной книги могло вызывать другое действие, необходимо аккаунту контракта дать на это разрешение. Для этого необходимо добавить cyber.code, исполнив: 
```
cleos set account permission addressbook active --add-code
```
Полномочия `cyber.code` позволяют контракту выполнять встроенные вызовы операций вида `action`.  

**7.2 Отправить уведомление**  
Открыть контракт addressbook.cpp и создать дополнительное действие, которое будет вести себя как транзакция, которая получает уведомление. Для этого можно создать вспомогательную функцию в классе адресной книги.

```cpp
[[eosio::action]]
void notify(name user, std::string msg) {}
```
Эта функция будет принимать только имя и строку.  

**7.3  Создать копию операции и переслать ее отправителю в качестве уведомления**  
Создать копию транзакции, в которой загружено действие, и отправить эту копию пользователю, чтобы ее можно было считать принятой. Для этого можно использовать метод `require_recipient`. Вызов `require_recipient` добавляет аккаунт в набор `require_recipient` и гарантирует, что данные аккаунты получают уведомление о выполняемом действии. Уведомление похоже на отправку аккаунтам «точной копии» действия.  

Чтобы исключить возможность несанкционированного вызова этой функции сторонним пользователем, необходимо требовать авторизацию для выполнения действия.  При этом авторизация должна предоставляться самим контрактом. Для этого нужно добавить метод `get_self()` для авторизации самого себя.
```cpp
[[eosio::action]]
  void notify(name user, std::string msg) {
    require_auth(get_self());
    require_recipient(user);
  }
```
При попытке вызова этой функции кем-либо, кроме самого контракта, будет сгенерировано исключение.  

**7.4 Уведомить вспомогательную функцию об отправке вызываемого действия**  
Поскольку с помощью встроенного вызова операция (action) может вызываться неоднократно, требуется создание вспомогательной функции для повторного использования кода с целью уменьшения создания дублирующего кода. 
```cpp
...
  private:
    void send_summary(name user, std::string message){}
```
Внутри этого помощника необходимо создать действие и отправить его.  

**7.5 Создать действие-конструктор**  
**7.5.1** Изменить контракт `addressbook`, чтобы отправлять квитанции пользователю при каждом вызове действия контракта. При этом нужно учитывать случай отсутствия записи в таблице.  

Сохранить этот объект в качестве уведомления `notification` для  действия.
```cpp
...
  private: 
    void send_summary(name user, std::string message){
      action(
        //permission_level,
        //code,
        //action,
        //data
      );   
    }
```
Требуется, чтобы в действии-конструкторе были следующие параметры:
  * структура уровня разрешения `permission_level`;
  * контракт для вызова (инициализируется с использованием типа `eosio::name`);
  * действие (инициализируется с использованием типа `eosio::name`);
  * данные, передаваемые действию.  

**7.5.2** Инициализировать структуру разрешения.  
В контракте разрешение должно быть авторизовано аккаунтом `active` контракта с помощью `get_self()`. Для использования встроенных «активных» полномочий необходимо, чтобы контракт предоставил активные полномочия коду `cyber.code`.
```cpp
...
  private: 
    void send_summary(name user, std::string message){
      action(
        permission_level{get_self(),"active"_n},
      );
    }
```  

**7.5.3** Задействовать `get_self()`.  
Поскольку вызываемое действие находится непосредственно в контракте, необходимо использовать `get_self()`. Также в нем будет работать адресная книга `"addressbook"_n`. Использование `get_self()` является наиболее приемлемым вариантом, так как контракт не будет работать в случае развертывания его под другим именем аккаунта.
```cpp
...
  private: 
    void send_summary(name user, std::string message){
      action(
        permission_level{get_self(),"active"_n},
        get_self(),
        //action
        //data
      );
    }
```  

**7.5.4** Задействовать оператор `_n`.
Действие `notify` было предварительно определено для вызова этого встроенного действия. Поэтому здесь необходимо использовать оператор `_n`.
```cpp
...
  private: 
    void send_summary(name user, std::string message){
      action(
        permission_level{get_self(),"active"_n},
        get_self(),
        "notify"_n,
        //data
      );
    }
```  

**7.5.5** Определить данные для передачи действию. 
Функция `notify` принимает два параметра — имя и строку. Действие-конструктор ожидает данные как тип `bytes`, поэтому необходимо использовать метод `make_tuple`, доступный через библиотеку `std C++`. Передаваемые данные зависят от их позиций и определяются порядком параметров, принимаемых вызываемым действием.  

Передать переменную `user`, указанную в качестве параметра действия `upsert()`.
Объединить строку, содержащую имя пользователя, и включить сообщение `message` для передачи уведомления `notify` в действие.
```cpp
...
  private: 
    void send_summary(name user, std::string message){
      action(
        permission_level{get_self(),"active"_n},
        get_self(),
        "notify"_n,
        std::make_tuple(user, name{user}.to_string() + message)
      );
    }
```  

**7.5.6** Отправить действие, используя метод `send`.
```cpp
...
  private: 
    void send_summary(name user, std::string message){
      action(
        permission_level{get_self(),"active"_n},
        get_self(),
        "notify"_n,
        std::make_tuple(user, name{user}.to_string() + message)
      ).send();
    }
```  

**7.6 Вызвать вспомогательную функцию и ввести соответствующее сообщение**  
К этому моменту помощник вспомогательная функция определена и может быть вызвана. Есть три события, когда может быть вызван новый помощник уведомлений `notify`:
  * После добавления контрактом новой записи: `send_summary (user, «успешно добавлена запись в адресную книгу»)`.
  * После изменения контрактом существующей записи: `send_summary (user, «успешно изменена запись в адресной книге»)`.
  * После удаления контрактом существующей записи: `send_summary (user, «успешно удалена запись из адресной книги»)`.  

**7.7 Внести изменение в макрос EOSIO_DISPATCH**  
Новое действие уведомления `notify` было добавлено в контракт, поэтому необходимо обновить макрос EOSIO_DISPATCH, чтобы включить это действие стало доступным для его выполнения извне (например, внешней транзакцией, вызовом операции (метода, действия). Это гарантирует, что действие `notify` не будет исключено оптимизатором `eosio.cdt`.
```cpp
EOSIO_DISPATCH( addressbook, (upsert)(erase)(notify) )
```
Контракт `addressbook` будет иметь следующий вид:
```cpp
#include <eosiolib/eosio.hpp>
#include <eosiolib/print.hpp>

using namespace eosio;

class [[eosio::contract]] addressbook : public eosio::contract {

public:
  using contract::contract;
  
  addressbook(name receiver, name code,  datastream<const char*> ds): contract(receiver, code, ds) {}

  [[eosio::action]]
  void upsert(name user, std::string first_name, std::string last_name, uint64_t age, std::string street, std::string city, std::string state) {
    require_auth(user);
    address_index addresses(_code, _code.value);
    auto iterator = addresses.find(user.value);
    if( iterator == addresses.end() )
    {
      addresses.emplace(user, [&]( auto& row ) {
       row.key = user;
       row.first_name = first_name;
       row.last_name = last_name;
       row.age = age;
       row.street = street;
       row.city = city;
       row.state = state;
      });
      send_summary(user, " successfully emplaced record to addressbook");
    }
    else {
      std::string changes;
      addresses.modify(iterator, user, [&]( auto& row ) {
        row.key = user;
        row.first_name = first_name;
        row.last_name = last_name;
        row.street = street;
        row.city = city;
        row.state = state;
      });
      send_summary(user, " successfully modified record to addressbook");
    }
  }

  [[eosio::action]]
  void erase(name user) {
    require_auth(user);

    address_index addresses(_self, _code.value);

    auto iterator = addresses.find(user.value);
    eosio_assert(iterator != addresses.end(), "Record does not exist");
    addresses.erase(iterator);
    send_summary(user, " successfully erased record from addressbook");
  }

  [[eosio::action]]
  void notify(name user, std::string msg) {
    require_auth(get_self());
    require_recipient(user);
  }

private:
  struct [[eosio::table]] person {
    name key;
    std::string first_name;
    std::string last_name;
    uint64_t age;
    std::string street;
    std::string city;
    std::string state;
  
  uint64_t get_secondary_1() const { return age;}
  
  };

  void send_summary(name user, std::string message) {
    action(
      permission_level{get_self(),"active"_n},
      get_self(),
      "notify"_n,
      std::make_tuple(user, name{user}.to_string() + message)
    ).send();
  };

  typedef eosio::multi_index<"people"_n, person, 
    indexed_by<"byage"_n, member<person, uint64_t, &person::age>>
  > address_index;
  
};

EOSIO_DISPATCH( addressbook, (upsert)(notify)(erase))
```

**7.8 Перекомпилировать и отредактировать ABI-файл**  
Перекомпилировать контракт с флагом `--abigen`, поскольку в контракт были внесены изменения, влияющие на ABI. 
```
cd CONTRACTS_DIR/addressbook
eosio-cpp -o addressbook.wasm addressbook.cpp --abigen
```

Контракты на EOSIO могут быть обновлены, поэтому данный контракт может быть перенесен с изменениями.
```
cleos set contract addressbook CONTRACTS_DIR/addressbook
```
В результате выполнения команды должна появиться информация вида:
```
Publishing contract...
executed transaction: ...
#         eosio <= eosio::setcode               {"account":"addressbook","vmtype":0,"vmversion":0,"code":"...
#         eosio <= eosio::setabi                {"account":"addressbook","abi":"...
```
**7.9 Тестирование**  
В предыдущем разделе запись адресной книги пользователя `alice` была удалена на этапе тестирования, поэтому при вызове `upsert` будет запущено встроенное действие.  

Исполнить:
```
cleos push action addressbook upsert '["alice", "alice", "liddell", 21, "123 drink me way", "wonderland", "amsterdam"]' -p alice@activ
```
В результате выполнения команды должна появиться информация вида:
```
executed transaction: ...
#   addressbook <= addressbook::upsert          {"user":"alice","first_name":"alice","last_name":"liddell","age":21,"street":"123 drink me way","cit...
#   addressbook <= addressbook::notify          {"user":"alice","msg":"alicesuccessfully emplaced record to addressbook"}
#         alice <= addressbook::notify          {"user":"alice","msg":"alicesuccessfully emplaced record to addressbook"}
```
Текст (внизу выдачи) сообщает, что адресная книга  уведомляет (`addressbook::notify`) пользователя `alice` о транзакции.  

Для просмотра выполненных действий, относящихся к пользователю `alice` можно использовать `cleos get actions`.
```
cleos get actions alice
```
Результатом выполнения команды будет информация вида:
```
#  seq  when     contract::action => receiver      trx id...   args
===================================================================
#   ...       addressbook::notify => alice         685ecc09... {"user":"alice","msg":"alice successfully added record to ad...
```