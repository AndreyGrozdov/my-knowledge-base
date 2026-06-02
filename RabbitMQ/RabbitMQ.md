# 🐇 Полный список команд RabbitMQ (шпаргалка)
## 🔧 Управление сервисом (systemd)
bash
sudo systemctl start rabbitmq-server      # запустить
sudo systemctl stop rabbitmq-server       # остановить
sudo systemctl restart rabbitmq-server    # перезапустить
sudo systemctl status rabbitmq-server     # статус
sudo systemctl enable rabbitmq-server     # автозагрузка
sudo systemctl disable rabbitmq-server    # убрать из автозагрузки
📊 Статус и диагностика
bash
sudo rabbitmqctl status                           # общий статус (версия, память, процессы)
sudo rabbitmqctl status | head -5                 # только первые строки (быстрая проверка)
sudo rabbitmq-diagnostics alarms                  # алармы (диск, память)
sudo rabbitmq-diagnostics memory_breakdown        # разбор потребления памяти
sudo rabbitmq-diagnostics disk_free                # свободное место на диске
sudo rabbitmq-diagnostics check_local_alarms       # локальные алармы
📋 Работа с очередями (Queues)
bash
## Список всех очередей с базовой информацией
sudo rabbitmqctl list_queues

## Очереди с количеством сообщений
sudo rabbitmqctl list_queues name messages_ready messages_unacknowledged

## Очереди с потребителями
sudo rabbitmqctl list_queues name consumers

## Полная информация об очереди
sudo rabbitmqctl list_queues name messages_ready messages_unacknowledged consumers memory

## Очистить очередь (удалить все сообщения)
sudo rabbitmqctl purge_queue имя_очереди

## Удалить очередь целиком
sudo rabbitmqctl delete_queue имя_очереди
📡 Работа с обменниками (Exchanges)
bash
## Список всех обменников
sudo rabbitmqctl list_exchanges

## Обменники с типом
sudo rabbitmqctl list_exchanges name type

## Создать обменник
sudo rabbitmqctl declare_exchange name=my_exchange type=direct

## Удалить обменник
sudo rabbitmqctl delete_exchange name=my_exchange
🔗 Работа с биндингами (Bindings)
bash
## Список всех биндингов
sudo rabbitmqctl list_bindings

## Привязать очередь к обменнику
sudo rabbitmqctl bind_queue name=queue_name exchange=exchange_name routing_key=key

## Отвязать очередь от обменника
sudo rabbitmqctl unbind_queue name=queue_name exchange=exchange_name routing_key=key
👥 Работа с пользователями
bash
## Список пользователей
sudo rabbitmqctl list_users

## Создать пользователя
sudo rabbitmqctl add_user username password

## Удалить пользователя
sudo rabbitmqctl delete_user username

## Сменить пароль
sudo rabbitmqctl change_password username new_password

## Очистить пароль (сделать невозможным вход)
sudo rabbitmqctl clear_password username
🔐 Права доступа
bash
## Назначить права на виртуальный хост "/"
sudo rabbitmqctl set_permissions -p / username ".*" ".*" ".*"

## Посмотреть права пользователя
sudo rabbitmqctl list_user_permissions username

## Очистить права пользователя
sudo rabbitmqctl clear_permissions -p / username

## Установить теги пользователя (администратор)
sudo rabbitmqctl set_user_tags username administrator

## Установить несколько тегов
sudo rabbitmqctl set_user_tags username administrator monitoring
🔌 Подключения и каналы
bash
## Список всех подключений
sudo rabbitmqctl list_connections

## Подключения с деталями (пользователь, хост, порт)
sudo rabbitmqctl list_connections user host port

## Закрыть подключение по PID
sudo rabbitmqctl close_connection pid "reason"

## Список каналов
sudo rabbitmqctl list_channels

## Каналы с неподтверждёнными сообщениями
sudo rabbitmqctl list_channels messages_unacknowledged
🎯 Потребители (Consumers)
bash
## Список всех потребителей
sudo rabbitmqctl list_consumers

## Потребители с очередями
sudo rabbitmqctl list_consumers queue_name
🚀 Кластеризация
bash
## Статус кластера
sudo rabbitmqctl cluster_status

## Присоединить ноду к кластеру (нода должна быть остановлена)
sudo rabbitmqctl stop_app
sudo rabbitmqctl join_cluster rabbit@node_name
sudo rabbitmqctl start_app

## Покинуть кластер (сбросить ноду)
sudo rabbitmqctl stop_app
sudo rabbitmqctl reset
sudo rabbitmqctl start_app

## Изменить тип ноды (disc → ram)
sudo rabbitmqctl stop_app
sudo rabbitmqctl change_cluster_node_type ram
sudo rabbitmqctl start_app
🧩 Плагины
bash
## Список всех плагинов
sudo rabbitmq-plugins list

## Включить плагин
sudo rabbitmq-plugins enable rabbitmq_management

## Выключить плагин
sudo rabbitmq-plugins disable rabbitmq_management

## Включить плагин без перезапуска
sudo rabbitmq-plugins enable --online rabbitmq_management
📦 Виртуальные хосты (vhosts)
bash
## Список всех виртуальных хостов
sudo rabbitmqctl list_vhosts

## Создать виртуальный хост
sudo rabbitmqctl add_vhost /new_vhost

## Удалить виртуальный хост
sudo rabbitmqctl delete_vhost /new_vhost

## Установить права пользователя на vhost
sudo rabbitmqctl set_permissions -p /vhost username ".*" ".*" ".*"
🔄 Федерация и шовелинг (продвинутые настройки)
bash
## Список федеративных связей
sudo rabbitmqctl list_federation_links

## Список шовелей
sudo rabbitmqctl list_shovels

## Создать шовель (через конфиг-файл, не CLI)
## Обычно настраивается через /etc/rabbitmq/advanced.config
📋 Политики (Policies)
bash
## Список политик
sudo rabbitmqctl list_policies

## Создать политику HA (высокая доступность)
sudo rabbitmqctl set_policy ha-all ".*" '{"ha-mode":"all"}' --apply-to queues

## Удалить политику
sudo rabbitmqctl clear_policy ha-all
🧪 Тестовые команды (для отладки)
bash
## Пинг ноды
sudo rabbitmqctl ping

## Выполнить произвольную команду Erlang
sudo rabbitmqctl eval 'node().'

## Посмотреть задержки между нодами
sudo rabbitmq-diagnostics erlang_cookie_distribution
📌 Ежедневный ритуал (6 команд для проверки здоровья)
bash
sudo rabbitmqctl status | head -5
sudo rabbitmqctl list_queues name messages_ready
sudo rabbitmqctl list_queues name consumers
sudo rabbitmqctl list_connections user host
sudo rabbitmqctl list_channels messages_unacknowledged
sudo rabbitmq-diagnostics alarms
✅ Что означают нормальные значения
Показатель	Норма	Проблема
messages_ready	0	> 0 и растёт
consumers	> 0 (при наличии сообщений)	0 при сообщениях
messages_unacknowledged	0	> 0 и не уменьшается
alarms	(пусто)	disk, memory
status	running	stopped
📖 Источник
Основано на официальной документации RabbitMQ CLI. Для более детального изучения:
man rabbitmqctl или официальная документация
