# Запуск

```shell
docker compose up -d
```

# Инициализация config server

```shell
docker compose exec configSrv mongosh --port 27017
```

```javascript
rs.initiate({
  _id: "config_server",
  configsvr: true,
  members: [
    { _id: 0, host: "configSrv:27017" }
  ]
})
```


# Инициализация shard1

```shell
docker compose exec shard1 mongosh --port 27018
```

```javascript
rs.initiate({
  _id: "shard1",
  members: [
    { _id: 0, host: "shard1:27018" }
  ]
})
```

# Инициализация shard2
```shell
docker compose exec shard2 mongosh --port 27019
```

```javascript
rs.initiate({
  _id: "shard2",
  members: [
    { _id: 0, host: "shard2:27019" }
  ]
})
```
# Добавление шардов
```shell
docker compose exec mongos_router mongosh --port 27020
```

```javascript   
sh.addShard("shard1/shard1:27018")
sh.addShard("shard2/shard2:27019")
```

# Включение шардирования
```javascript
sh.enableSharding("somedb")
sh.shardCollection("somedb.helloDoc", { name: "hashed" })
``` 

# Заполнение базы
``` javascript
use somedb

for (let i = 0; i < 1000; i++) {
  db.helloDoc.insertOne({ age: i, name: "ly" + i })
}
```

# Проверка

```javascript
db.helloDoc.countDocuments()    
``` 
