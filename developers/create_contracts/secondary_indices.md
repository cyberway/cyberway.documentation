
# 6 Вторичные индексы
В этом разделе приведено описание сортировки таблиц по индексам на примере таблицы `voteinfo`, а также инструкции по добавлению индекса в таблицу на примере контракта `addressbook`.  

### 6.1 Формат таблицы и индексов

В EOS для каждого индекса создается структура, которая возвращает значение. В CyberWay для описания индекса либо создается отдельная функция и при этом используется `const_mem_fun` в описании таблицы, либо используется поле из структуры  `eosio::member`.  

#### Требование к формату таблицы
Описание таблицы должно соответствовать следующему формату:   
```
using vote_table = multi_index<N(vote), structures::voteinfo, vote_id_index, vote_messageid_index, vote_group_index>
```
Параметры:  
`N(vote)` — первичный индекс;  
`vote_table` — имя таблицы;  
`voteinfo` — имя структуры;  
`vote_id_index` — индекс первичного ключа (`primary key`);  
`vote_messageid_index` — индекс, идентифицирующий сообщение. Для каждого голоса при голосовании формируется сообщение, которому присваивается номер. По этому номеру в таблице отыскивается сообщение;  
`vote_group_index` — вторичный индекс, составной.  

В таблице всегда должны быть:
  * первичный индекс (тип `uint64`);
  * определенное количество вторичных индексов, которые могут быть описаны как по одному полю с использованием `by_message`, так и по нескольким полям.  

Описание вторичного индекса по одному полю имеет вид:
```
    uint64_t by_message() const {
        return message_id;
    }
```
При описании вторичного индекса по нескольким полям указываются следующие параметры:
  * шаблон индекса `by_`;
  * имя индекса;
  * описание индекса:
    * `const_mem_fun` — описание в простом виде;
    * `eosio::member` — описание в сложном виде.

При описании вторичного индекса в сложном виде `eosio::member` указываются `composite_key`, имя структуры и перечень полей, из которых формируется индекс (например, из полей `message_id` и `voter`).
```
      eosio::member<structures::voteinfo, uint64_t, &structures::voteinfo::message_id>,
      eosio::member<structures::voteinfo, name, &structures::voteinfo::voter>>>;
```
#### Требование к описанию вторичных индексов  
Описание вторичных индексов должно совпадать с их описанием в ABI-файле. При этом должен быть соблюден порядок расположения, а также соответствие типов. Сериализация и десериализация передаваемых данных в бинарном виде должны выполняться одинаковым способом.  

В зависимости от назначения вторичных индексов существуют два способа их описания:  
  * описание для работы внутри контракта;
  * описание для работы вне контракта. При этом описание таблицы с индексами находится в ABI-файле.

#### Описание вторичного индекса для работы внутри контракта
Описание полей `name` и `info` аналогично описанию в EOS. Отличие от EOS состоит в том, что вместо полей индексов `indexed_name` и  `indexed_type`, используемых в EOS, в CyberWay используются описание индексов.  

Индекс представляет собой массив, в котором каждому индексу присваивается имя. Это имя должно совпадать с именем, указанным в шаблоне `indexed_by`. Совпадение идет по следующим признакам:  
  * `N(id)` — совпадение по порядковому номеру параметра. Например, N1 — выбор первичного индекса;  
  * `N(messageid)` — совпадение по порядковому номеру сообщения. Каждому голосу во время голосования присваивается номер сообщения;  
  * `N(byvoter)` — совпадение по порядковому номеру голосуемого.

```
using vote_id_index = indexed_by<N(id), const_mem_fun<structures::voteinfo, uint64_t, &structures::voteinfo::primary_key>>;

using vote_messageid_index = indexed_by<N(messageid), const_mem_fun<structures::voteinfo, uint64_t, &structures::voteinfo::by_message>>;

using vote_group_index = indexed_by<N(byvoter), eosio::composite_key<structures::voteinfo, 
      eosio::member<structures::voteinfo, uint64_t, &structures::voteinfo::message_id>,
      eosio::member<structures::voteinfo, name, &structures::voteinfo::voter>>>;
```
#### Описание вторичного индекса для работы вне контракта
В описание индекса введено понятие уникальности «unique» — будет ли значение этого индекса для записи уникальным или нет. Значение `unique=true` означает уникальность.  

Сортировка  обеспечивает упорядоченное расположение индексов. 
```
          "orders": [
              {"field": "id", "order": "asc"}
```
В поле «orders» указываются все поля, которые будут отсортированы.
Описание таблицы для смарт-контракта указывается в ABI-файле.
Есть порядок, в котором описываются поля индексов (`messageid` и `voter`). Значения этих полей должны следовать в том порядке, в котором они содержатся в `composite_key`, а именно в `eosio::member` или `const_mem_ func`.
Второй атрибут `order` определяет способ построения индекса внутри базы данных. Этот параметр определяет порядок появления элементов, если эти элементы извлекаются из таблицы один за другим, то есть определяет правило, по которому находится очередной элемент в таблице.  

Пример описания таблицы с индексами в ABI-файле:
```
  {
    "name": "vote",
    "type": "voteinfo",
    "indexes": [
       {
          "name": "primary",
          "unique": "true",
          "orders": [
              {"field": "id", "order": "asc"}
          ]
       },
       {
          "name": "messageid",
          "unique": "false",
          "orders": [
              {"field": "message_id", "order": "asc"}
          ]
       },
       {
          "name": "byvoter",
          "unique": "true",
          "orders": [
              {"field": "message_id", "order": "asc"},
              {"field": "voter", "order": "asc"}
          ]
       }
    ]
  },
```


#### Пример использования индексов внутри смарт-контракта  
```
    auto votetable_index = vote_table.get_index<"messageid"_n>();
    auto vote_itr = votetable_index.lower_bound(mssg_itr->id);
    while ((vote_itr != votetable_index.end()) && (vote_itr->message_id == mssg_itr->id))
        vote_itr = votetable_index.erase(vote_itr);
```
Вторичный индекс используется из одного поля, то есть `messageid` состоит из одного поля.  
`lower_bound` — используется для поиска нижней границы по указанному ID. От найденной нижней границы индекс проходит по всем голосам, пока голос считается отданным за тот же самый пост.

#### Пример использования составного вторичного индекса  
```
 auto votetable_index = vote_table.get_index<"byvoter"_n>();
    auto vote_itr = votetable_index.find(std::make_tuple(mssg_itr->id, voter));
```
`byvoter` — состоит из двух полей, передаваемых через `make_tuple`. Используется для поиска голоса, относящегося к сообщению `mssg_itr->id` проголосовавшего пользователя `voter`. 

#### Функции, используемые для поиска записей в таблице  
  * создание (emplace); 
  * удаления (erase);
  * изменение (modify);
  * поиск (find, lower_bound, upper_bound).
Также для выполнения операций необходим индекс, определяющий способ поиска записей в таблице.

### 6.2 Пример добавления индекса в таблицу контракта addressbook
В предыдущем разделе рассмотрены инструкции по созданию и настройке таблицы `addressbook`. Далее приводятся инструкции по добавлению индекса в данную тавлицу.  

**6.2.1 Удалить существующие записи из таблицы**
Структура таблицы не может быть изменена, если она содержит данные. Необходимо удалить уже имеющиеся в ней записи пользователей `alice` и `bob`.
```
cleos push action addressbook erase '["alice"]' -p alice@active
cleos push action addressbook erase '["bob"]' -p bob@active
```

**6.2.2 Добавить новое поле для индекса к контракту addressbook.cpp**  
Поскольку вторичный индекс должен быть числовым полем, добавляется переменная типа uint64_t.

```cpp
uint64_t age;
```

**6.2.3  Добавить вторичный индекс для таблицы addresses** 
Поле определено как вторичный индекс. Необходимо переконфигурировать таблицу `address_index`.
  
```cpp
typedef eosio::multi_index<"people"_n, person, 
indexed_by<"byage"_n, member<person, uint64_t, &person::get_secondary_1>>
 > address_index;
```
`index_by` — структура, которая используется для создания экземпляра индекса.
`«byage» — оператор вызова функции, который извлекает значение const в качестве ключа индекса.

Таблица с несколькими индексами будет индексировать записи по переменной `age`.
```cpp
indexed_by<"byage"_n, member<person, uint64_t, &person::get_secondary_1>>
```

**6.2.4 Обновить параметры функции upsert**  
С учетом всех изменений функция будет иметь вид:  

```cpp
 void upsert(
name user, std::string first_name, std::string last_name, uint64_t age, std::string street, std::string city, std::string state)
```
Добавить дополнительные строки для обновления поля `age` в функции `upsert` следующим образом:
```cpp
 void upsert(name user, std::string first_name, std::string last_name, uint64_t age, std::string street, std::string city, std::string state) {
    require_auth( user );
    address_index addresses( get_first_receiver(), get_first_receiver().value);
    auto iterator = addresses.find(user.value);
    if( iterator == addresses.end() )
    {
      addresses.emplace(user, [&]( auto& row ) {
       row.key = user;
       row.first_name = first_name;
       row.last_name = last_name;
       // -- Add code below --
       row.age = age; 
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
        // -- Add code below --
        row.age = age;
        row.street = street;
        row.city = city;
        row.state = state;
      });
    }
  }
```


**6.2.5 Скомпилировать и развернуть контракт addressbook**  
```
eosio-cpp -o addressbook.wasm addressbook.cpp --abigen
cleos set contract addressbook CONTRACTS_DIR/addressbook
```
**6.2.6 Тестирование**
Вставить записи в таблицу пользователей `alice` и `bob`.
```
cleos push action addressbook upsert '["alice", "alice", "liddell", 9, "123 drink me way", "wonderland", "amsterdam"]' -p alice@active
cleos push action addressbook upsert '["bob", "bob", "is a guy", 49, "doesnt exist", "somewhere", "someplace"]' -p bob@active
```
Проверить данные таблицы для пользователей `alice` и `bob` по индексу `age`.  В командных строках использовать параметр `--index 2` для указания запроса по вторичному индексу (индекс № 2).
```
cleos get table addressbook addressbook people --upper 10 --key-type i64 --index 2
cleos get table addressbook addressbook people --upper 50 --key-type i64 --index 2
```

Поиск данных таблицы по вторичному индексу считается успешно выполненным, если будет получена информация вида:

```
{
  "rows": [{
      "key": "alice",
      "first_name": "alice",
      "last_name": "liddell",
      "age": 9,
      "street": "123 drink me way",
      "city": "wonderland",
      "state": "amsterdam"
    },{
      "key": "bob",
      "first_name": "bob",
      "last_name": "is a loser",
      "age": 49,
      "street": "doesnt exist",
      "city": "somewhere",
      "state": "someplace"
    }
  ],
  "more": false
}
```