# 5 Перечень команд, применяемых к любому виду контейнера
Ниже приведен перечень команд, которые могут быть использованы в работе с любым видом контейнера.  

**Запуск контейнера**  
Установление соединения с `docker` и запуск контейнера:  
```
sudo docker exec -ti nodeosd /bin/bash
```

**Получение текста протокола**  
Получение текста лог-файла о контейнере:
```
sudo docker logs --tail 10 -f nodeosd
```

**Подключение к узлу**  
Подключение через cleos к узлу (ноде) для проверки его успешного функционирования (для выполнения данной инструкции необходимо, чтобы был сконфигурирован кошелек wallet):
sudo docker exec -ti nodeosd \
    /opt/cyberway/bin/cleos

**Запуск контейнеров**  
Команда запуска контейнеров (команда выполняется из директории, в которой находится файл docker-compose.yml):  
```cpp
sudo docker start nodeosd mongo
```

**Останов контейнеров**  
Команда останова контейнеров  (команда выполняется из директории, в которой находится файл docker-compose.yml):
```cpp
sudo docker stop nodeosd mongo
```