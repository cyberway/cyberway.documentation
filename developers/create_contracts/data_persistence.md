
# 5 Хранение данных пользователя
В настоящем разделе приведены инструкции по созданию и настройки таблицы для данных пользователя, по созданию экземпляра таблицы, добавлению новых строк, а также инструкции по их изменению и удалению. Кроме этого приведены инструкции по настройке ABI-файла и тестированию установленного контракта.  

Для хранения данных пользователя может быть создан контракт, функционирующий как адресная книга. Несмотря на то, что этот вариант использования данных не очень практичен в качестве продуктового контракта, создание адресной книги позволяет наиболее наглядно ознакомить разработчика с методикой хранения данных в CyberWay без отвлечения на бизнес-логику.  

**5.1 Создать новую директорию для контракта адресной книги**  
```
cd CONTRACTS_DIR
mkdir addressbook
cd addressbook
```
**5.2 Создать файл**  
```
touch addressbook.cpp
```
Наполнить файл следующим содержимым:
```cpp
#include <eosiolib/eosio.hpp>

using namespace eosio;

class [[eosio::contract("addressbook")]] addressbook : public eosio::contract {
    public:
       
    private: 
  
};

```  

**5.3 Создать структуру данных адресной книги**  
Перед тем, как таблица может быть настроена и создана, необходимо сформировать структуру данных адресной книги. Поскольку это адресная книга, таблица должна содержать информацию о людях, а сам контракт должен хранить некоторые важные данные для каждой записи или персоны. Вариант структуры `person` имеет вид:

```cpp
struct person {
    name key;
    std::string first_name;
    std::string last_name;
    std::string street;
    std::string city;
    std::string state
};
```
  
> Структура таблицы не может быть изменена, пока в ней есть данные. В случае необходимости внести изменения в структуру данных таблицы, предварительно требуется удалить все ее строки.

**5.4 Конфигурирование индексов таблицы**  
После определения структуры данных таблицы необходимо настроить таблицу. В первую очередь необходимо определить конструктор `eosio::multi_index` для использования структуры данных таблицы. В созданный класс добавить строку вида:
```
typedef eosio::multi_index<"people"_n, person> address_index;
```
Данное определение multi_index создает таблицу с именем `people`, которая позволяет следующее:  
  * использовать оператор `_n` для определения типа `eosio::name` и также использовать это имя для таблицы. Данная таблица будет содержать несколько описаний отдельных «person», поэтому таблице можно присвоить имя «people»;  
  * передать структуру для отдельного лица, определенную в предыдущем шаге;  
  * объявить тип этой таблицы. Этот тип будет использован для создания экземпляров этой таблицы;  
  * сконфигурировать и настроить индексы таблицы, которые будут рассмотрены далее.  

На данном этапе файл должен иметь следующий вид:
```cpp
#include <eosiolib/eosio.hpp>

using namespace eosio;

class [[eosio::contract("addressbook")]] addressbook : public eosio::contract {

  public:

  private:
    struct [[eosio::table]] person {
      name key;
      std::string first_name;
      std::string last_name;
      std::string street;
      std::string city;
      std::string state
    };
  
    typedef eosio::multi_index<"people"_n, person> address_index;
  public:
  
};
```

**5.5 Создание конструктора класса**  
При работе с классами C ++ первым открытым методом, который должен быть создан, является конструктор. Конструктор определяет первоначальную настройку контракта. Контракты позволяют расширить класс. Для этого необходимо инициализировать родительский класс контракта с кодовым именем контракта и получателя.
```
addressbook(name receiver, name code, datastream<const char*> ds):contract(receiver, code, ds) {}
```
Параметр:  
`name code` — имя аккаунта контракта.  


**5.6  Создание записи в таблице**  

**5.6.1** Необходимо обеспечить выполнение следующих условий:  
  * Только сам пользователь может внести изменение в адресную книгу.  
  * Для удобства использования контракт должен предоставлять возможность создавать или изменять строку таблицы одним действием.  

**5.6.2** Определить действие (action) пользователя для добавления записи или изменения таблицы. Это действие должно принимать любые значения, необходимые для создания или редактирования записи в таблице.  

Отформатировать метод. Для удобства пользователя и простоты интерфейса один и тот же метод будет выполнять как создание так и изменение строк. Для удобства ему присваивается имя «upsert» (сочетание «update» и «insert»).  

Только пользователь имеет право контролировать свою собственную запись в таблице. Для этого используется метод `require_auth`, предоставляемый `eosio.cdt`. Этот метод принимает один аргумент — имя аккаунта, выполняющего транзакцию. Метод `upsert` имеет вид:
```cpp
void upsert(
    name user, 
    std::string first_name, 
    std::string last_name, 
    std::string street, 
    std::string city, 
    std::string state
) {
  require_auth( user );
}
```

**5.6.3** Создать таблицу. 
Таблица `multi_index` настроена и объявлена как `address_index`. Для создания экземпляра таблицы требуются два обязательных аргумента:  
  * код аккаунта контракта;  
  * область видимости контракта.  
Также требуется установить итератор,  который будет использоваться неоднократно. 
Таблица будет иметь вид:

```cpp
void upsert(
    name user, 
    std::string first_name, 
    std::string last_name, 
    std::string street, 
    std::string city, 
    std::string state) {
        require_auth( user );
        address_index addresses(_code, _code.value);
        auto iterator = addresses.find(user.value);
}
```

**5.6.4** Написать логику создания или изменения таблицы, логику для определения существования пользователя.  
Для этого можно использовать метод `find` таблицы, передав параметр user. Метод `find` возвращает итератор таблицы.
```cpp
void upsert(
… 
) {
    require_auth( user );
    address_index addresses(_code, _code.value);
    auto iterator = addresses.find(user.value);
    if( iterator == addresses.end() )
  	{
   		 //The user isn't in the table
 	}
  	else {
 		//The user is in the table
 	 }
}
```

**5.6.5** Создать запись в таблице, используя метод `emplace`. Этот метод принимает два аргумента: «плательщик» этой записи, который оплачивает использование хранилища, и функцию обратного вызова.  

Функция обратного вызова для метода `emplace` должна использовать лямбду для создания ссылки. Внутри тела присвоить значения строкам для `upsert`.
```cpp
void upsert(
…
) {
    require_auth( user );
    address_index addresses(_code, _code.value);
    auto iterator = addresses.find(user.value);
    if( iterator == addresses.end() )
    {
        addresses.emplace(user, [&]( auto& row ) {
        row.key = user;
        row.first_name = first_name;
        row.last_name = last_name;
        row.street = street;
        row.city = city;
        row.state = state;});
    }
    else {
        //The user is in the table
    }
}
```

**5.6.6** Для изменения записи в таблице можно использовать метод `modify` со следующими аргументами:  
  * iterator — итератор (ранее определенный);  
  * user — пользователь, являющийся плательщиком;  
  * Функция обратного вызова для обработки модификации таблицы.
```cpp
 ...
if( iterator == addresses.end() )
{
    ...
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
}
...
```
По завершении выполнения инструкций данного пункта контракт адресной книги будет иметь функциональное действие, которое позволит пользователю создать или изменить строку в таблице.  

**5.7 Удаление записи из таблицы**  
Cоздать публичный метод в адресной книге, убедившись, что в него включены объявления ABI. Также создать `require_auth` для проверки права пользователя на выполнение действия. Только владелец записи может изменить свой аккаунт. В адресной книге каждый аккаунт имеет только одну запись. 

Для удаления записи необходимо установить итератор с помощью `find` и вызвать метод `erase`
```cpp
...
void erase(name user) {
    require_auth(user);
    address_index addresses(_code, _code.value);
    auto iterator = addresses.find(user.value);
    eosio_assert(iterator != addresses.end(), "Record does not exist");
    addresses.erase(iterator);
}
...
```

По завершении выполнения инструкций данного пункта контракт адресной книги будет иметь функциональное действие, которое позволит пользователю создавать, изменять или удалять запись в таблице.Тем не менее, контракт еще не готов к компиляции.  

**5.8 Отредактировать ABI-файл**  

Перед тем как скомпилировать контракт необходимо отредактировать `addressbook.cpp` файл.  

**5.8.1** В конце файла поместить макрос EOSIO_DISPATCH, указав имя контракта, а также действия «upsert» и «erase».  

Этот макрос необходим для отправки вызовов к конкретным методам в этом контракте. Макрос обеспечивает совместимость cpp-файла с интерпретатором wasm EOSIO.
Отказ от включения этой декларации может привести к ошибке при развертывании контракта.
```
EOSIO_DISPATCH( addressbook, (upsert)(erase) )
```

**5.8.2** Объявить действия в ABI-файле.  
Несмотря на то, что eosio.cdt включает в себя генератор ABI, требуется также в ручном режиме внести изменения в сгенерированный ABI-файл.
В верхней части описания функций  `upsert` и `erase` добавить объявление вида:
```
[[eosio::action]]
```
Это объявление позволяет извлечь аргументы действия и создать необходимые описания структуры ABI в сгенерированном ABI-файле.  

**5.8.3** Объявить таблицы в ABI-файле.  
Аналогично инструкции предыдущего пункта изменить объявление для таблицы:
``` 
struct [[eosio::table]] person {
```
Окончательное состояние контракта с адресной книгой перед компиляцией имеет вид:
```cpp
#include <eosiolib/eosio.hpp>

using namespace eosio;

class [[eosio::contract("addressbook")]] addressbook : public eosio::contract {

private:
  struct [[eosio::table]] person {
	name key;
	std::string first_name;
	std::string last_name;
	std::string street;
	std::string city;
	std::string state;
 
	uint64_t primary_key() const {return key.value;}
  };
  typedef eosio::multi_index<"person"_n, person> address_index;

public:
  using contract::contract;
  
addressbook(name receiver, name code,  datastream<const char*> ds): contract(receiver, code, ds) {}
 
  [[eosio::action]]
  void upsert(name user, std::string first_name, std::string last_name, std::string street,std::string city, std::string state) {
        require_auth( user );
        address_index addresses(_self, _self.value);
        auto iterator = addresses.find(user.value);
        if( iterator == addresses.end() )
        {
            addresses.emplace(user, [&]( auto& row ) {
                row.key = user;
                row.first_name = first_name;
                row.last_name = last_name;
                row.street = street;
                row.city = city;
                row.state = state;
            });
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
      }
  }
 
  [[eosio::action]]
  void erase(name user) {
    require_auth(user);
 
    address_index addresses(_self, _self.value);
 
    auto iterator = addresses.find(user.value);
    eosio_assert(iterator != addresses.end(), "Record does not exist");
    addresses.erase(iterator);
  }
 
};
 
EOSIO_DISPATCH( addressbook, (upsert)(erase))
```

**5.9 Скомпилировать контракт**  
```
eosio-cpp -o addressbook.wasm addressbook.cpp --abigen
```

**5.10 Описать индекс первичного ключа таблицы в ABI-файле**  
Во время компиляции ABI-файла, генератор создает таблицу без индексов:
```
  ...
  "tables": [
  {
      "name": "person",
      "type": "person",
      "index_type": "i64",
      "key_names": [],
      "key_types": []
  }],
  …
```
Необходимо отредактировать таблицу в ABI-файле, сохранив неизменным только `name` и `type` и заменив остальные поля на следующее описание индекса первичного ключа:
```
  …
    "indexes": [
    {
        "name": "primary",
        "unique": "true",
        "orders": [
        {"field": "key", "order": "asc"}
   ]}
  …
```

**5.11** Установить контракт**  

**5.11.11** Создать аккаунт для контракта, исполнив:
```
cleos create account olga addressbook <public key>
```

**5.11.12** Установить addressbook контракт, исполнив: 
```
cleos set contract addressbook CONTRACTS_DIR/addressbook -p addressbook@active
```

**5.12 Протестировать установленный контракт**   

**5.12.1** Проверить добавление строки в таблицу для пользователя с именем `alice`.
```
cleos push action addressbook upsert '["alice", "alice", "liddell", "123 drink me way", "wonderland", "amsterdam"]' -p alice@active
```
В результирующей выдаче должна появиться информация вида: 
```
executed transaction: ...
#   addressbook <= addressbook::upsert          {"user":"alice","first_name":"alice","last_name":"liddell","street":"123 drink me way","city":"wonde...
```

**5.12.2** Проверить, что пользователь `alice` не может создать запись для другого пользователя, исполнив:
```
cleos push action addressbook upsert '["bob", "bob", "is a loser", "doesnt exist", "somewhere", "someplace"]' -p alice@active
```
Метод `require_auth` в контракте не должен позволить создавать (или изменять) строки другого пользователя. В результате должно появиться сообщение об ошибке:
```
Error 3090004: Missing required authority
Ensure that you have the related authority inside your transaction!;
If you are currently using 'cleos push action' command, try to add the relevant authority using -p option.
```

**5.12.3** Получить запись пользователя с именем Алисы.
```
cleos get table addressbook addressbook people --lower alice --limit 1
```
В результирующей выдаче должна появиться информация вида:
```
{
  "rows": [{
      "key": "3773036822876127232",
      "first_name": "alice",
      "last_name": "liddell",
      "street": "123 drink me way",
      "city": "wonderland",
      "state": "amsterdam"
    }
  ],
  "more": false
}
```

**5.12.4** Проверить, что пользователь с именем `alice` может удалить запись из таблицы, исполнив:
```
cleos push action addressbook erase '["alice"]' -p alice@active
```
В результирующей выдаче должна появиться информация вида:
```
executed transaction: … 
#   addressbook <= addressbook::erase           {"user":"alice"}
warning: transaction executed locally, but may not be confirmed by the network yet    ]
```
Проверить отсутствие записи, исполнив:
```
cleos get table addressbook addressbook people --lower alice --limit 1
```
Должна появиться информация вида:
```
{
  "rows": [],
  "more": false
}
```