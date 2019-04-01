
# 8 Обращение к контракту через встроенные (inline) действия

В этом разделе руководства приведены инструкции по отправке действий во внешний контракт. Примеры выполнения инструкций показаны на контракте, в котором ведется  счет выполняемых действий, написанных в этом контракте.  

**8.1 Контракт счетчика адресной книги addressbook**   
Войти в `CONTRACTS_DIR` и создать там каталог с именем `abcounter`, а также файл `abcounter.cpp`.
```
cd CONTRACTS_DIR
mkdir abcounter
touch abcounter.cpp
```
Открыть файл `abcounter.cpp` и записать в него следующий программный текст:
```cpp
#include <eosiolib/eosio.hpp>

using namespace eosio;

class [[eosio::contract]] abcounter : public eosio::contract {
  public:
    using contract::contract;

    abcounter(name receiver, name code,  datastream<const char*> ds): contract(receiver, code, ds) {}

    [[eosio::action]]
    void count(name user, std::string type) {
      require_auth( name("addressbook"));
      count_index counts(name(_code), _code.value);
      auto iterator = counts.find(user.value);
      
      if (iterator == counts.end()) {
        counts.emplace("addressbook"_n, [&]( auto& row ) {
          row.key = user;
          row.emplaced = (type == "emplace") ? 1 : 0;
          row.modified = (type == "modify") ? 1 : 0;
          row.erased = (type == "erase") ? 1 : 0;
        });
      }
      else {
        counts.modify(iterator, "addressbook"_n, [&]( auto& row ) {
          if(type == "emplace") { row.emplaced += 1; }
          if(type == "modify") { row.modified += 1; }
          if(type == "erase") { row.erased += 1; }
        });
      }
    }

  private:
    struct [[eosio::table]] counter {
      name key;
      uint64_t emplaced;
      uint64_t modified;
      uint64_t erased;
    };

    using count_index = eosio::multi_index<"counts"_n, counter>;
};

EOSIO_DISPATCH( abcounter, (count));
```
Особенность данного тексте в том, что в нем имеется ограничение на вызовы действия для аккаунта контракта. Для ограничения используется `require_auth` для контракта `addressbook`. Только аккаунт контракта `addressbook` имеет авторизацию на выполнение `require_auth`. 
```cpp
//Only the addressbook account/contract can authorize this command. 
require_auth( name("addressbook"));
```

**8.2 Создать аккаунт для  контракта abcounter**  
```
cleos create account cyber abcounter YOUR_PUBLIC_KEY
```

**8.3 Скомпилировать и установить контракт abcounter**  
```
eosio-cpp -o abcounter.wasm abcounter.cpp --abigen
cleos set contract abcounter CONTRACTS_DIR/abcounter
```

**8.4 Модифицировать контракт addressbook для отправки внутреннего (inline) действия в новый контракт abcounter**  
```
cd CONTRACTS_DIR/addressbook
```
Открыть файл `addressbook.cpp` и создать в нем другого помощника с именем `increment_counter` в части `private` контракта.
```cpp
void increment_counter(name user, std::string type) {
    
  action counter = action(
    permission_level{get_self(),"active"_n},
    "abcounter"_n,
    "count"_n,
    std::make_tuple(user, type)
  );

  counter.send();
}
```
В теле действия содержатся:
  * выдача разрешения уровня `active` .Для разрешения функция `get_self()` возвращает текущий контракт `addressbook`;  
  * имя `abcounter` аккаунта контракта;  
  * действие `count` для вызова;  
  * данные, имя пользователя и тип строки.

Добавить вызовы «установить», «модифицировать» и «стереть»  помощникам в их области видимости действия.
```cpp
//Emplace
increment_counter(user, "emplace");
//Modify
increment_counter(user, "modify");
//Erase
increment_counter(user, "erase");
```
В завершении файл `addressbook.cpp` должен иметь следующий вид:
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

       send_summary(user, " successfully emplaced record to addressbook");
       increment_counter(user, "emplace");
      });
    }
    else {
      std::string changes;
      addresses.modify(iterator, user, [&]( auto& row ) {
        row.key = user;
        row.first_name = first_name;
        row.last_name = last_name;
        row.age = age;
        row.street = street;
        row.city = city;
        row.state = state;

        send_summary(user, " successfully modified record to addressbook");
        increment_counter(user, "modify");
      });

    }
  }

  [[eosio::action]]
  void erase(name user) {
    require_auth(user);

    address_index addresses(_code, _code.value);
    auto iterator = addresses.find(user.value);
    eosio_assert(iterator != addresses.end(), "Record does not exist");
    addresses.erase(iterator);
    send_summary(user, " successfully erased record from addressbook");
    increment_counter(user, "erase");
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

  void increment_counter(name user, std::string type) {
    
    action counter = action(
      permission_level{get_self(),"active"_n},
      "abcounter"_n,
      "count"_n,
      std::make_tuple(user, type)
    );

    counter.send();
  }

  typedef eosio::multi_index<"people"_n, person, 
    indexed_by<"byage"_n, member<person, uint64_t, &person::get_secondary_1>>
  > address_index;
  
};

EOSIO_DISPATCH( addressbook, (upsert)(notify)(erase))
```

**8.5 Скомпилировать заново и установить контракт addressbook**  
 Поскольку изменения не должны повлиять на ABI, повторно восстанавливать ABI-файл не требуется (убедиться, что этот файл не обновился).
```
eosio-cpp -o addressbook.wasm addressbook.cpp
cleos set contract addressbook CONTRACTS_DIR/addressbook
```

**8.6 Выполнить тестирование отправки действия из контракта addressbook контракту abcounter**  

Перед началом тестирования убедиться, что контракты `abcounter` и `addressbook` установлены.  

**8.6.1** Проверить отправку уведомления контракту `abcounter`, а также изменение записи в таблице контракта `addressbook`, исполнив: 
```
cleos push action addressbook upsert '["alice", "alice", "liddell", 19, "123 drink me way", "wonderland", "amsterdam"]' -p alice@active
```
Действие считается успешно выполненным, если в результирующей выдаче будет содержаться информация вида:
```
executed transaction: ...
#   addressbook <= addressbook::upsert          {"user":"alice","first_name":"alice","last_name":"liddell","street":"123 drink me way","city":"wonde...
#   addressbook <= addressbook::notify          {"user":"alice","msg":"alice successfully modified record in addressbook"}
#         alice <= addressbook::notify          {"user":"alice","msg":"alice successfully modified record in addressbook"}
#     abcounter <= abcounter::count             {"user":"alice","type":"modify"}
```
**8.6.2** Проверить появление строки в таблице для пользователя `alice`.
```
cleos get table abcounter abcounter counts --lower alice --limit 1
```
Действие считается успешно выполненным, если в результате будет получена информация вида:
```
{
  "rows": [{
      "key": "alice",
      "emplaced": 1,
      "modified": 0,
      "erased": 0
    }
  ],
  "more": false
}
```

**8.6.3** Проверить, что метод `upsert` модифицирует запись.
```
cleos push action addressbook upsert '["alice", "alice", "liddell", 21,"1 there we go", "wonderland", "amsterdam"]' -p alice@active
```
Действия считаются успешно выполненными, если в результирующей выдаче будет содержаться информация вида:
```
executed transaction: …
#   addressbook <= addressbook::upsert          {"user":"alice","first_name":"alice","last_name":"liddell","street":"1 coming down","city":"normalla...
#   addressbook <= addressbook::notify          {"user":"alice","msg":"alice successfully emplaced record to addressbook"}
>> Notified
#         alice <= addressbook::notify          {"user":"alice","msg":"alice successfully emplaced record to addressbook"}
#     abcounter <= abcounter::count             {"user":"alice","type":"emplace"}
warning: transaction executed locally, but may not be confirmed by the network yet    ]
```

**8.6.4** Проверить удаление записи для пользователя `alice` из таблицы, исполнив:
```
cleos push action addressbook erase '["alice"]' -p alice@active
```
Действие считается успешно выполненной, если в результирующей выдаче будет содержаться информация вида:
```
executed transaction: ...
#   addressbook <= addressbook::erase           {"user":"alice"}
>> Erased
#   addressbook <= addressbook::notify          {"user":"alice","msg":"alice successfully erased record from addressbook"}
>> Notified
#         alice <= addressbook::notify          {"user":"alice","msg":"alice successfully erased record from addressbook"}
#     abcounter <= abcounter::count             {"user":"alice","type":"erase"}
warning: transaction executed locally, but may not be confirmed by the network yet    ]
Toaster:addressbook sandwich$
```
**8.6.5** Проверить возможность манипулировать данными контракта `abcounter`, исполнив:
```
cleos push action abcounter count '["alice", "erase"]' -p alice@active
cleos get table abcounter abcounter counts --lower alice
``` 
Таблица контракта `abcounter` должна содержать следующую информацию:
```
{
  "rows": [{
      "key": "alice",
      "emplaced": 1,
      "modified": 1,
      "erased": 1
    }
  ],
  "more": false
}
```