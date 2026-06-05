# Просмотр всех очередей
curl -u admin:SBRFOdintsovo2026 http://localhost:15672/api/queues

# Создать очередь
curl -u admin:SBRFOdintsovo2026 -X PUT http://localhost:15672/api/queues/%2F/queue_name -H "Content-Type: application/json" -d '{"durable":true}'

# Отправить сообщение
curl -u admin:SBRFOdintsovo2026 -X POST http://localhost:15672/api/exchanges/%2F/amq.default/publish -H "Content-Type: application/json" -d '{"routing_key":"queue_name","payload":"text"}'

# Забрать сообщение
curl -u admin:SBRFOdintsovo2026 -X POST http://localhost:15672/api/queues/%2F/queue_name/get -H "Content-Type: application/json" -d '{"count":1,"requeue":false}'

# Удалить очередь
curl -u admin:SBRFOdintsovo2026 -X DELETE http://localhost:15672/api/queues/%2F/queue_name
