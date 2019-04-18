## BALANCES
Коллекция BALANCES содержит документы с данными о состоянии батареек, выделенных пользователям. Пример документа:
```
{
    "_id" : ObjectId("5cb5783b710a09490e7e45bd"),
    "charge_symbol" : NumberDecimal("91600047785729"),
    "token_code" : "GOLOS",
    "charge_id" : NumberDecimal("1"),
    "last_update" : NumberDecimal("1577837382000000"),
    "value" : NumberLong(7982),
    "_SERVICE_" : { ... }
}
```
**Параметры**   
* `charge_symbol` — символ токена батарейки.  
* `token_code` — код токена, имеющего привязку к батарейке `charge_id`.  
* `charge_id` — идентификатор батарейки.  
* `last_update` — время последнего обращения к батарейке.  
* `value` — значение заряда батарейки. 