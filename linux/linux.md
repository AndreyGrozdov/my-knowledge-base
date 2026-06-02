# 🐧 Полный список команд Linux (шпаргалка)

## 🔧 Основные команды для навигации
```
pwd                                  # показать текущую директорию
ls                                   # список файлов и папок
ls -l                                # подробный список (права, владелец, размер)
ls -a                                # показать скрытые файлы (.файлы)
ls -la                               # подробно + скрытые
ls -lh                               # подробно + читаемые размеры (K, M, G)
ls -ltr                              # подробно + сортировка по времени (старые сверху)
ls -R                                # рекурсивный список (все подпапки)
cd /path/to/dir                      # перейти в директорию
cd ~                                 # перейти в домашнюю директорию
cd ..                                # перейти на уровень вверх
cd -                                 # вернуться в предыдущую директорию
```

## 📁 Работа с файлами и папками
```
touch file.txt                       # создать пустой файл или обновить дату
mkdir dir_name                       # создать папку
mkdir -p path/to/dir                 # создать папку с промежуточными (если нет)
cp source.txt dest.txt               # скопировать файл
cp -r source_dir dest_dir            # рекурсивно скопировать папку
cp -p source.txt dest.txt            # скопировать с сохранением прав/даты
mv old_name.txt new_name.txt         # переименовать файл
mv file.txt /path/to/dest/           # переместить файл
rm file.txt                          # удалить файл
rm -r dir_name                       # рекурсивно удалить папку
rm -rf dir_name                      # принудительно удалить папку (без запросов)
rmdir dir_name                       # удалить пустую папку
```

## 📄 Просмотр файлов
```
cat file.txt                         # показать весь файл
head file.txt                        # показать первые 10 строк
head -20 file.txt                    # показать первые 20 строк
tail file.txt                        # показать последние 10 строк
tail -20 file.txt                    # показать последние 20 строк
tail -f file.txt                     # следить за файлом в реальном времени (лог)
less file.txt                        # постраничный просмотр (q - выход)
more file.txt                        # постраничный просмотр (пробел - дальше)
tac file.txt                         # показать файл в обратном порядке (снизу вверх)
```

## 🔍 Поиск файлов и текста
```
find /path -name "*.txt"             # найти файлы по имени
find /path -type f -size +100M       # найти файлы больше 100 МБ
find /path -type f -mtime -7         # файлы изменённые за последние 7 дней
find /path -type f -mtime +30        # файлы изменённые более 30 дней назад
find /path -type d -name "logs"      # найти папки с именем logs
grep "text" file.txt                 # искать текст в файле
grep -r "text" /path/                # рекурсивно искать текст во всех файлах
grep -i "text" file.txt              # искать без учёта регистра
grep -v "text" file.txt              # показать строки НЕ содержащие текст
grep -n "text" file.txt              # показать номера строк
grep -A 5 "text" file.txt            # показать 5 строк ПОСЛЕ совпадения
grep -B 5 "text" file.txt            # показать 5 строк ДО совпадения
grep -C 5 "text" file.txt            # показать 5 строк ВОКРУГ совпадения
locate filename                      # быстрый поиск файла (обновить: updatedb)
```

## 🔧 Права доступа и владельцы
```
ls -l file.txt                       # показать права (например -rw-r--r--)
chmod 755 file.txt                   # установить права (числовой способ)
chmod +x script.sh                   # добавить право на выполнение (символьный способ)
chmod u+x,g-w,o=r file.txt           # изменить права явно
chown user:group file.txt            # изменить владельца и группу
chown -R user:group dir/             # рекурсивно изменить владельца для папки
chgrp group file.txt                 # изменить только группу
umask 027                            # установить маску прав по умолчанию
```

## 📊 Процессы
```
ps aux                               # все процессы (подробно)
ps -ef                               # все процессы (альтернативный формат)
ps aux | grep process_name           # найти процесс по имени
pgrep process_name                   # найти PID процесса по имени
pidof process_name                   # найти PID процесса (ещё точнее)
top                                  # мониторинг процессов в реальном времени
htop                                 # интерактивный мониторинг (цветной)
kill PID                             # завершить процесс (SIGTERM - мягко)
kill -9 PID                          # принудительно завершить процесс (SIGKILL)
kill -15 PID                         # корректно завершить процесс (SIGTERM)
kill -HUP PID                        # перечитать конфигурацию (без остановки)
pkill process_name                   # завершить процесс по имени
pkill -9 process_name                # принудительно по имени
killall process_name                 # завершить все процессы с именем
nice -n 10 command                   # запустить с низким приоритетом
renice -n 5 -p PID                   # изменить приоритет запущенного процесса
nohup command &                      # запустить процесс, игнорируя закрытие терминала
```

## 💾 Системная информация
```
uname -a                             # информация о системе (ядро, ОС, архитектура)
uname -r                             # версия ядра
cat /etc/os-release                  # информация об ОС (дистрибутив, версия)
hostname                             # имя хоста
hostname -f                          # полное доменное имя (FQDN)
uptime                               # время работы системы и нагрузка
whoami                               # текущий пользователь
id                                   # информация о пользователе (UID, GID, группы)
who                                  # кто залогинен в системе
w                                    # кто залогинен и что делает
last                                 # последние логины
last -10                             # последние 10 логинов
lastb                                # неудачные попытки входа
history                              # история команд
history -c                           # очистить историю команд
```

## 💿 Диски и память
```
df -h                                # свободное место на дисках (читаемый формат)
df -i                                # количество свободных inodes
du -sh /path                         # размер папки
du -sh *                             # размер всех файлов/папок в текущей папке
du -h --max-depth=1 /var             # размер папок на уровне 1 (сортировка)
free -h                              # свободная оперативная память
free -m                              # свободная память в МБ
swapon -s                            # информация о swap-разделе
vmstat 1                             # статистика памяти и CPU (каждую секунду)
iostat -x 1                          # статистика дисковых операций (каждую секунду)
lsblk                                # список блочных устройств (диски и разделы)
fdisk -l                             # таблица разделов (требует sudo)
mount                                # список примонтированных файловых систем
umount /mount/point                  # отмонтировать файловую систему
```
## 🌐 Сеть
```
ip a                                 # IP-адреса интерфейсов (альтернатива ifconfig)
ip r                                 # таблица маршрутизации
ss -tlnp                             # слушающие TCP порты (с процессами)
ss -ulnp                             # слушающие UDP порты
ss -tnp                              # активные TCP соединения
netstat -tlnp                        # слушающие порты (устаревшая команда)
ping google.com                      # проверить доступность хоста
ping -c 4 google.com                 # отправить 4 пакета и остановиться
traceroute google.com                # отследить маршрут до хоста
mtr google.com                       # комбинация ping + traceroute (в реальном времени)
nslookup google.com                  # DNS-запрос
dig google.com                       # детальный DNS-запрос
curl -I http://localhost             # получить HTTP заголовки
curl -v http://localhost             # детальный вывод (с заголовками и телом)
curl -X POST -d "data" url          # отправить POST запрос
curl -k https://untrusted.com       # игнорировать SSL сертификат
wget http://file.zip                 # скачать файл
scp user@host:/path/file ./         # скопировать файл с удалённого сервера
nc -zv google.com 80                 # проверить, открыт ли порт 80
tcpdump -i eth0 -n port 80           # захватить трафик на порту 80
tcpdump -i any -c 100                # первые 100 пакетов
```
## 🔐 Управление пользователями
```
sudo useradd username                # создать пользователя
sudo useradd -m -s /bin/bash username  # с домашней папкой и bash
sudo passwd username                 # сменить пароль
sudo userdel username                # удалить пользователя
sudo userdel -r username             # удалить пользователя и его домашнюю папку
sudo usermod -aG group username      # добавить пользователя в группу
sudo usermod -s /bin/bash username   # изменить оболочку входа
sudo groupadd groupname              # создать группу
sudo groupdel groupname              # удалить группу
groups username                      # показать группы пользователя
id username                          # показать UID, GID, группы
```

## 🔑 sudo и права
```
sudo command                         # выполнить команду от root
sudo -i                              # стать root (полная сессия)
sudo -u username command             # выполнить команду от другого пользователя
sudo -l                              # показать права sudo для текущего пользователя
visudo                               # редактировать /etc/sudoers (безопасно)
```

## 📦 Пакетный менеджер (apt - Debian/Ubuntu)
```
sudo apt update                      # обновить список пакетов
sudo apt upgrade                     # обновить все пакеты
sudo apt full-upgrade                # полное обновление (с удалением)
sudo apt install nginx               # установить пакет
sudo apt remove nginx                # удалить пакет (оставить конфиги)
sudo apt purge nginx                 # полностью удалить пакет (с конфигами)
sudo apt autoremove                  # удалить ненужные зависимости
apt search nginx                     # найти пакет
apt show nginx                       # показать информацию о пакете
apt list --installed                 # список всех установленных пакетов
dpkg -l                              # список пакетов (альтернативный)
dpkg -S /usr/bin/nginx               # найти, из какого пакета файл
```

## 📦 Пакетный менеджер (yum/dnf - CentOS/RHEL)
```
sudo yum update                      # обновить пакеты (CentOS 7)
sudo yum install nginx               # установить пакет
sudo yum remove nginx                # удалить пакет
sudo yum search nginx                # найти пакет
sudo yum list installed              # список установленных пакетов
sudo dnf update                      # обновить пакеты (CentOS 8+)
sudo dnf install nginx               # установить пакет (CentOS 8+)
sudo dnf remove nginx                # удалить пакет (CentOS 8+)
rpm -qf /usr/bin/nginx               # найти, из какого пакета файл
```

## 📦 Пакетный менеджер (zypper - SUSE)
```
sudo zypper refresh                  # обновить список пакетов
sudo zypper update                   # обновить пакеты
sudo zypper install nginx            # установить пакет
sudo zypper remove nginx             # удалить пакет
sudo zypper search nginx             # найти пакет
```

## 📝 Работа с текстом (обработка)
```
echo "text"                          # вывести текст
cat file1 file2 > file3              # объединить файлы в один
head -20 file.txt | tail -5          # строки 15-20 (с 16 по 20)
wc -l file.txt                       # количество строк в файле
wc -w file.txt                       # количество слов
sort file.txt                        # отсортировать строки
sort -r file.txt                     # отсортировать в обратном порядке
sort -n file.txt                     # числовая сортировка
uniq file.txt                        # убрать дубликаты (требует сортировки)
uniq -c file.txt                     # показать количество дубликатов
cut -d: -f1 /etc/passwd              # вырезать первое поле (разделитель :)
awk '{print $1}' file.txt            # вывести первую колонку
awk -F: '{print $1, $3}' /etc/passwd # использовать : как разделитель
sed 's/old/new/g' file.txt           # заменить текст в файле
sed -i 's/old/new/g' file.txt        # заменить в том же файле (без создания нового)
diff file1.txt file2.txt             # сравнить файлы
comm file1.txt file2.txt             # сравнить файлы (столбцами)
```

## 🗜️ Архивация и сжатие
```
tar -czvf archive.tar.gz /path       # создать архив .tar.gz
tar -xzvf archive.tar.gz             # распаковать .tar.gz
tar -cjvf archive.tar.bz2 /path      # создать архив .tar.bz2
tar -xjvf archive.tar.bz2            # распаковать .tar.bz2
tar -xvf archive.tar                 # распаковать .tar
tar -tvf archive.tar.gz              # посмотреть содержимое архива
gzip file.txt                        # сжать в .gz (оригинал удаляется)
gzip -k file.txt                     # сжать с сохранением оригинала
gunzip file.txt.gz                   # распаковать .gz
zcat file.txt.gz                     # показать содержимое сжатого файла
zgrep "text" file.txt.gz             # искать в сжатом файле
bzip2 file.txt                       # сжать в .bz2
bunzip2 file.txt.bz2                 # распаковать .bz2
zip archive.zip file.txt             # создать .zip
unzip archive.zip                    # распаковать .zip
```

## 🔁 Перенаправление и конвейеры
```
command > file.txt                   # перенаправить вывод в файл (перезаписать)
command >> file.txt                  # перенаправить вывод (добавить в конец)
command 2> error.txt                 # перенаправить ошибки
command > file.txt 2>&1              # перенаправить вывод и ошибки в один файл
command1 | command2                  # передать вывод command1 в command2
command1 && command2                 # выполнить command2, если command1 успешно
command1 || command2                 # выполнить command2, если command1 провалился
```

## 🎯 Управление задачами (jobs)
```
command &                            # запустить команду в фоне
jobs                                 # список фоновых задач
fg %1                                # вернуть задачу 1 на передний план
bg %1                                # продолжить выполнение задачи в фоне
Ctrl+Z                               # приостановить текущую задачу
Ctrl+C                               # прервать текущую задачу
Ctrl+D                               # выход из терминала (EOF)
Ctrl+R                               # поиск по истории команд (reverse-i-search)
```

## 🔗 Символические ссылки
```
ln -s /path/to/original link_name    # создать символическую ссылку
ln /path/to/original link_name       # создать жёсткую ссылку
ls -la                               # посмотреть ссылки (-> показывает куда ведут)
unlink link_name                     # удалить ссылку
```

## 🖥️ Переменные окружения
```
echo $PATH                           # показать PATH
export VAR="value"                   # создать переменную окружения
echo $VAR                            # показать переменную
env                                  # показать все переменные окружения
unset VAR                            # удалить переменную
export PATH=$PATH:/new/path          # добавить путь в PATH
```

## 👤 Пользовательские файлы конфигурации
```
~/.bashrc                            # конфигурация bash (запускается при входе)
~/.bash_profile                      # профиль bash (запускается при входе)
~/.bash_logout                       # выполняется при выходе
~/.bash_history                      # история команд
/etc/bashrc                          # глобальная конфигурация bash
/etc/profile                         # глобальный профиль
source ~/.bashrc                     # перечитать конфигурацию (без перезахода)
```


