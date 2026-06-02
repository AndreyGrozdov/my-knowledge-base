🐳 Полный список команд Docker (шпаргалка)

🔧 Управление сервисом Docker

sudo systemctl start docker      # запустить Docker
sudo systemctl stop docker       # остановить Docker
sudo systemctl restart docker    # перезапустить Docker
sudo systemctl status docker     # статус Docker
sudo systemctl enable docker     # автозагрузка
sudo systemctl disable docker    # убрать из автозагрузки

📦 Управление образами (Images)

docker images                    # список образов
docker image ls                  # список образов (альтернативный)
docker pull nginx:latest         # скачать образ из реестра
docker push username/repo:tag    # загрузить образ в реестр
docker build -t myapp .          # собрать образ из Dockerfile
docker rmi nginx:latest          # удалить образ
docker rmi -f nginx:latest       # принудительно удалить образ
docker image prune               # удалить неиспользуемые образы
docker image prune -a            # удалить все неиспользуемые образы
docker save -o backup.tar myapp  # сохранить образ в tar-файл
docker load -i backup.tar        # загрузить образ из tar-файла
docker tag myapp:latest myapp:v1 # создать тег для образа
docker history nginx:latest      # показать историю слоёв образа
docker inspect nginx:latest      # показать детальную информацию об образе

🚀 Управление контейнерами (Containers)

docker ps                        # список запущенных контейнеров
docker ps -a                     # список всех контейнеров
docker run nginx:latest          # запустить контейнер из образа
docker run -d nginx:latest       # запустить в фоновом режиме (detach)
docker run -it ubuntu bash       # запустить с интерактивным терминалом
docker run --name web -p 80:80 nginx:latest   # с именем и пробросом порта
docker run --rm nginx:latest     # удалить контейнер после остановки
docker run -v /host:/container nginx:latest   # примонтировать том
docker run --env VAR=value nginx:latest       # передать переменную окружения
docker stop container_name       # остановить контейнер
docker start container_name      # запустить остановленный контейнер
docker restart container_name    # перезапустить контейнер
docker kill container_name       # принудительно остановить контейнер
docker rm container_name         # удалить контейнер
docker rm -f container_name      # принудительно удалить контейнер
docker rm $(docker ps -aq)       # удалить все остановленные контейнеры
docker pause container_name      # приостановить контейнер
docker unpause container_name    # возобновить контейнер
docker wait container_name       # ждать завершения контейнера
docker rename old_name new_name  # переименовать контейнер

📝 Просмотр логов и информации

docker logs container_name       # показать логи контейнера
docker logs -f container_name    # следить за логами (follow)
docker logs --tail 100 container_name   # последние 100 строк
docker logs --since 2024-01-01 container_name   # логи с определённой даты
docker inspect container_name    # детальная информация о контейнере
docker stats                     # статистика использования ресурсов (live)
docker stats container_name      # статистика конкретного контейнера
docker top container_name        # процессы внутри контейнера
docker port container_name       # порты контейнера
docker diff container_name       # изменения в файловой системе контейнера
docker events                    # события Docker в реальном времени
docker events --filter event=start   # фильтровать события
docker system df                 # использование дискового пространства
docker system info               # информация о системе Docker
docker version                   # версия Docker

🔄 Взаимодействие с контейнером

docker exec container_name ls    # выполнить команду в контейнере
docker exec -it container_name bash   # открыть терминал внутри контейнера
docker cp file.txt container_name:/path/   # скопировать файл в контейнер
docker cp container_name:/path/file.txt . # скопировать файл из контейнера
docker attach container_name     # подключиться к запущенному контейнеру

📂 Управление томами (Volumes)

docker volume ls                 # список томов
docker volume create volume_name # создать том
docker volume inspect volume_name   # информация о томе
docker volume rm volume_name     # удалить том
docker volume prune              # удалить неиспользуемые тома
docker run -v volume_name:/path nginx  # использовать том

🌐 Управление сетями (Networks)

docker network ls                # список сетей
docker network create mynet      # создать сеть
docker network inspect mynet     # информация о сети
docker network rm mynet          # удалить сеть
docker network prune             # удалить неиспользуемые сети
docker run --network mynet nginx # запустить контейнер в сети
docker network connect mynet container_name   # подключить контейнер к сети
docker network disconnect mynet container_name   # отключить контейнер от сети

🗑️ Очистка системы

docker system prune              # удалить остановленные контейнеры, неиспользуемые сети, образы
docker system prune -a           # удалить всё неиспользуемое (включая образы без тегов)
docker system prune -f           # принудительная очистка без подтверждения
docker container prune           # удалить все остановленные контейнеры
docker image prune               # удалить неиспользуемые образы
docker image prune -a            # удалить все неиспользуемые образы
docker volume prune              # удалить неиспользуемые тома
docker network prune             # удалить неиспользуемые сети

🏗️ Docker Compose

docker-compose up                # запустить сервисы
docker-compose up -d             # запустить в фоновом режиме
docker-compose down              # остановить и удалить контейнеры
docker-compose down -v           # остановить и удалить контейнеры и тома
docker-compose ps                # статус сервисов
docker-compose logs              # логи всех сервисов
docker-compose logs service_name # логи конкретного сервиса
docker-compose logs -f           # следить за логами
docker-compose build             # пересобрать образы
docker-compose build --no-cache  # пересобрать без кэша
docker-compose pull              # обновить образы
docker-compose restart           # перезапустить все сервисы
docker-compose stop              # остановить все сервисы
docker-compose start             # запустить все сервисы
docker-compose exec service_name bash   # выполнить команду в сервисе
docker-compose config            # проверить и показать конфигурацию
docker-compose config -q         # проверить конфигурацию (тихо)
docker-compose up --scale web=3  # масштабировать сервис

🔐 Docker Registry

docker login                     # войти в реестр
docker logout                    # выйти из реестра
docker search nginx              # поиск образов в реестре
docker pull nginx:latest         # скачать образ
docker push username/repo:tag    # загрузить образ

📋 Dockerfile инструкции (для напоминания)

FROM ubuntu:22.04                # базовый образ
RUN apt update && apt install -y nginx   # выполнить команду при сборке
COPY ./app /app                  # скопировать файлы
ADD ./app.tar /app               # скопировать файлы (поддерживает архивы)
WORKDIR /app                     # рабочая директория
ENV NODE_ENV=production          # переменная окружения
EXPOSE 80                        # открыть порт
CMD ["nginx", "-g", "daemon off;"]   # команда по умолчанию (exec форма)
CMD nginx -g daemon off;         # команда по умолчанию (shell форма)
ENTRYPOINT ["nginx"]             # точка входа (exec форма)
USER www-data                    # пользователь для запуска
VOLUME ["/data"]                 # создать том

✅ Что означают нормальные значения

Показатель                 Норма          Проблема
контейнеры в docker ps     0-несколько    слишком много (утечка)
docker ps -a               ожидаемое      неожиданно остановленные
docker images              известные      много неиспользуемых образов
место на диске docker system df < 80%       > 80% требуется очистка
docker stats               CPU/память     перегрузка контейнера
                           в норме

📖 Источник

Основано на официальной документации Docker CLI.
Для более детального изучения: man docker
или официальная документация: https://docs.docker.com/engine/reference/commandline/docker/
