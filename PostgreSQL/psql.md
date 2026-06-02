# 🐘 Полный список команд PostgreSQL (шпаргалка)

## 🔧 Управление сервисом PostgreSQL

```
sudo systemctl start postgresql      # запустить PostgreSQL
sudo systemctl stop postgresql       # остановить PostgreSQL
sudo systemctl restart postgresql    # перезапустить PostgreSQL
sudo systemctl status postgresql     # статус PostgreSQL
sudo systemctl enable postgresql     # автозагрузка
sudo systemctl disable postgresql    # убрать из автозагрузки
```

## 🔌 Подключение к PostgreSQL
```
sudo -u postgres psql                # подключиться от пользователя postgres (локально)
psql -h host -p 5432 -U user -d db   # удалённое подключение
psql -U username -d database         # подключиться с указанием пользователя и БД
psql -d database                     # подключиться к конкретной базе
psql -c "SELECT version()"           # выполнить команду и выйти
psql -f script.sql                   # выполнить SQL-скрипт из файла
PGPASSWORD="pass" psql -U user -d db # подключиться с паролем (небезопасно)
```

## 📋 Базовые команды внутри psql
```
\l                                   # список всех баз данных
\c database_name                     # подключиться к базе данных
\dt                                  # список таблиц в текущей базе
\dt schema.*                         # список таблиц в указанной схеме
\du                                  # список пользователей
\dn                                  # список схем
\dx                                  # список установленных расширений
\d table_name                        # структура таблицы (колонки, типы, индексы)
\d+ table_name                       # расширенная структура таблицы
\di                                  # список индексов
\dv                                  # список представлений (views)
\ds                                  # список последовательностей (sequences)
\df                                  # список функций
\df+                                 # список функций с деталями
\sf function_name                    # показать код функции
\du                                  # список ролей (пользователей)
\dp                                  # список прав доступа к таблицам
\z                                   # то же, что \dp
\?                                   # справка по командам psql
\h CREATE TABLE                      # справка по SQL команде
\q                                   # выход из psql
\! ls -la                            # выполнить команду оболочки из psql
```

## 📊 Информация о базах данных
```
SELECT version();                    # версия PostgreSQL
SELECT current_database();           # текущая база данных
SELECT current_user;                 # текущий пользователь
SELECT now();                        # текущая дата и время
\l+                                  # список баз данных с размером (в psql)
\l                                   # список баз данных (в psql)
```
## 📦 Размеры баз и таблиц
```
SELECT pg_database_size(current_database())/1024/1024 AS size_mb;  # размер текущей БД (МБ)
SELECT pg_database_size('db_name')/1024/1024 AS size_mb;           # размер конкретной БД (МБ)
SELECT pg_total_relation_size('table_name')/1024/1024 AS size_mb;  # размер таблицы + индексы (МБ)
SELECT pg_relation_size('table_name')/1024/1024 AS size_mb;        # размер таблицы без индексов (МБ)
SELECT pg_size_pretty(pg_database_size(current_database()));       # размер в читаемом виде
SELECT pg_size_pretty(pg_total_relation_size('table_name'));       # размер таблицы в читаемом виде
```

## 📊 Мониторинг активности
```
SELECT * FROM pg_stat_activity;                                   # все подключения
SELECT * FROM pg_stat_activity WHERE state = 'active';            # активные подключения
SELECT pid, usename, application_name, client_addr, state, query FROM pg_stat_activity;  # основные поля
SELECT count(*) FROM pg_stat_activity;                            # количество подключений
SELECT usename, count(*) FROM pg_stat_activity GROUP BY usename;  # подключения по пользователям
SELECT datname, count(*) FROM pg_stat_activity GROUP BY datname;  # подключения по базам данных
SELECT pg_terminate_backend(pid);                                 # убить подключение по PID
SELECT pg_cancel_backend(pid);                                    # отменить текущий запрос (мягко)
```

## 📈 Статистика запросов (требуется расширение pg_stat_statements)
```
CREATE EXTENSION IF NOT EXISTS pg_stat_statements;                 # включить расширение
SELECT query, calls, total_time, mean_time FROM pg_stat_statements ORDER BY total_time DESC LIMIT 10;  # топ медленных запросов
```
## 🔐 Управление пользователями и ролями
```
\du                                  # список пользователей (в psql)
SELECT usename FROM pg_user;         # список пользователей
CREATE USER username WITH PASSWORD 'password';                    # создать пользователя
CREATE ROLE rolename WITH LOGIN PASSWORD 'password';              # создать роль
ALTER USER username WITH PASSWORD 'new_password';                 # сменить пароль
ALTER USER username RENAME TO new_username;                       # переименовать пользователя
DROP USER username;                                               # удалить пользователя
GRANT CONNECT ON DATABASE dbname TO username;                     # дать право подключения к БД
GRANT ALL PRIVILEGES ON DATABASE dbname TO username;              # дать все права на БД
GRANT SELECT, INSERT, UPDATE ON table_name TO username;           # дать права на таблицу
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO username;  # дать права на все таблицы
REVOKE ALL PRIVILEGES ON DATABASE dbname FROM username;           # отозвать права
```

## 🗂️ Управление базами данных
```
CREATE DATABASE dbname;                                           # создать базу данных
CREATE DATABASE dbname OWNER username;                            # создать БД с владельцем
CREATE DATABASE dbname ENCODING 'UTF8';                           # создать БД с кодировкой
DROP DATABASE dbname;                                             # удалить базу данных
ALTER DATABASE dbname RENAME TO new_dbname;                       # переименовать БД
ALTER DATABASE dbname OWNER TO new_owner;                         # сменить владельца БД
```

## 📋 Управление таблицами
```
CREATE TABLE table_name (id SERIAL PRIMARY KEY, name TEXT, created_at TIMESTAMP DEFAULT NOW());  # создать таблицу
DROP TABLE table_name;                                            # удалить таблицу
DROP TABLE IF EXISTS table_name;                                  # удалить таблицу (без ошибки)
TRUNCATE table_name;                                              # очистить таблицу (быстро)
TRUNCATE table_name RESTART IDENTITY;                             # очистить и сбросить последовательности
ALTER TABLE table_name ADD COLUMN new_column TEXT;                # добавить колонку
ALTER TABLE table_name DROP COLUMN column_name;                   # удалить колонку
ALTER TABLE table_name RENAME COLUMN old_name TO new_name;        # переименовать колонку
ALTER TABLE table_name RENAME TO new_name;                        # переименовать таблицу
ALTER TABLE table_name ALTER COLUMN column_name TYPE INTEGER;     # изменить тип колонки
ALTER TABLE table_name SET SCHEMA new_schema;                     # переместить таблицу в другую схему
```

## 📑 Работа с данными (CRUD)
```
INSERT INTO table_name (name) VALUES ('value');                   # вставить одну строку
INSERT INTO table_name (name) VALUES ('value1'), ('value2');      # вставить несколько строк
INSERT INTO table_name (name) SELECT name FROM other_table;       # вставить из выборки
SELECT * FROM table_name;                                         # выбрать все данные
SELECT name, id FROM table_name WHERE id = 1;                     # выбрать с условием
SELECT * FROM table_name ORDER BY name DESC;                      # сортировка
SELECT * FROM table_name LIMIT 10;                                # ограничить количество строк
SELECT * FROM table_name OFFSET 10;                               # пропустить строки
UPDATE table_name SET name = 'new_value' WHERE id = 1;            # обновить данные
DELETE FROM table_name WHERE id = 1;                              # удалить строку
DELETE FROM table_name;                                           # удалить все строки
```

## 🔍 Индексы
```
CREATE INDEX idx_name ON table_name (column_name);                # создать индекс
CREATE UNIQUE INDEX idx_name ON table_name (column_name);         # создать уникальный индекс
CREATE INDEX idx_name ON table_name (column1, column2);           # составной индекс
DROP INDEX idx_name;                                              # удалить индекс
REINDEX INDEX idx_name;                                           # перестроить индекс
REINDEX DATABASE dbname;                                          # перестроить все индексы в БД
```
## 🔗 Схемы (Schemas)
```
CREATE SCHEMA schema_name;                                        # создать схему
DROP SCHEMA schema_name;                                          # удалить схему
DROP SCHEMA schema_name CASCADE;                                  # удалить схему со всем содержимым
SET search_path TO schema_name, public;                           # установить схему по умолчанию
ALTER SCHEMA schema_name OWNER TO new_owner;                      # сменить владельца схемы
\dn                                  # список схем (в psql)
```
## 🔄 Резервное копирование и восстановление
```
pg_dump dbname > backup.sql                                       # бэкап базы данных
pg_dump -U username dbname > backup.sql                           # бэкап с указанием пользователя
pg_dump -h host -p 5432 -U user dbname > backup.sql               # удалённый бэкап
pg_dump -t table_name dbname > backup.sql                         # бэкап конкретной таблицы
pg_dump --schema-only dbname > schema.sql                         # только структура (без данных)
pg_dump --data-only dbname > data.sql                             # только данные (без структуры)
pg_dumpall > all_backup.sql                                       # бэкап всех баз данных
pg_dumpall --globals-only > globals.sql                           # бэкап пользователей и глобальных объектов
psql dbname < backup.sql                                          # восстановление базы
psql -f backup.sql                                                # восстановление (указав файл)
pg_restore -d dbname backup.dump                                  # восстановление из custom формата
pg_restore --clean -d dbname backup.dump                          # восстановить с удалением существующих объектов
```

## 📊 Настройка и конфигурация (через SQL)
```
SHOW all;                                                         # показать все настройки
SHOW config_file;                                                 # путь к файлу конфигурации
SHOW data_directory;                                              # путь к директории с данными
SHOW port;                                                        # показать порт
SHOW max_connections;                                             # показать максимальное количество подключений
ALTER SYSTEM SET max_connections = '200';                         # изменить параметр
ALTER SYSTEM RESET max_connections;                               # сбросить параметр к значению по умолчанию
SELECT pg_reload_conf();                                          # перезагрузить конфигурацию (без перезапуска)
```

## 📁 Основные файлы конфигурации (вне psql)
```
/etc/postgresql/версия/main/postgresql.conf                        # основные настройки (Linux)
/var/lib/pgsql/версия/data/postgresql.conf                         # основные настройки (CentOS/RHEL)
/etc/postgresql/версия/main/pg_hba.conf                            # настройки клиентской аутентификации
/var/lib/pgsql/версия/data/pg_hba.conf                             # настройки аутентификации (CentOS/RHEL)
/etc/postgresql/версия/main/pg_ident.conf                          # отображение имён пользователей
```

## 🔧 Восстановление пароля (если забыли)
```
sudo -u postgres psql                                             # войти от postgres
ALTER USER username WITH PASSWORD 'new_password';                 # сменить пароль
\q                                                                # выйти
```

## 📊 Полезные расширения
```
CREATE EXTENSION IF NOT EXISTS pg_stat_statements;                # статистика запросов
CREATE EXTENSION IF NOT EXISTS pgcrypto;                          # криптографические функции
CREATE EXTENSION IF NOT EXISTS uuid-ossp;                         # генерация UUID
CREATE EXTENSION IF NOT EXISTS postgis;                           # геоданные (PostGIS)
\dx                                  # список установленных расширений
DROP EXTENSION extension_name;                                    # удалить расширение
```

## ✅ Что означают нормальные значения
```
Показатель                 Норма          Проблема
статус сервиса PostgreSQL  active         inactive, failed
логин через psql           успешный       FATAL, password authentication failed
\l                         базы видны     no relations found
\dt                        таблицы видны  No relations found
pg_stat_activity           немного записей много записей (утечка соединений)
размер БД                  стабильный     быстро растёт (утечка данных)
SELECT запрос              результат      timeout, ERROR, deadlock
pg_dump                    файл создан    Permission denied, no space left
ALTER SYSTEM               успешно        не хватает прав, invalid setting
```
