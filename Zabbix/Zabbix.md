## 🎯 Основные метрики (ключи) Zabbix агента
```
agent.ping                                       # проверка доступности (1 - OK)
system.hostname                                  # имя хоста
system.uname                                     # информация о системе (uname -a)
system.cpu.load[all,avg1]                        # load average за 1 минуту
system.cpu.util[,user]                           # использование CPU в user mode
system.cpu.util[,system]                         # использование CPU в system mode
system.cpu.util[,idle]                           # idle CPU
vm.memory.size[total]                            # общий объём RAM (байт)
vm.memory.size[available]                        # доступная RAM (байт)
vm.memory.size[free]                             # свободная RAM (байт)
vfs.fs.size[/,total]                             # общий размер раздела (байт)
vfs.fs.size[/,free]                              # свободное место на разделе (байт)
vfs.fs.size[/,used]                              # использованное место на разделе (байт)
vfs.fs.inode[/,total]                            # общее количество inodes
vfs.fs.inode[/,free]                             # свободные inodes
net.if.in[eth0]                                  # входящий трафик (байт/с)
net.if.out[eth0]                                 # исходящий трафик (байт/с)
net.tcp.listen[80]                               # слушает ли порт 80 (1 - да)
net.tcp.port[192.168.1.1,80]                     # доступен ли порт 80 на указанном IP
proc.num[nginx]                                  # количество процессов nginx
log[/var/log/messages]                           # мониторинг лог-файла
logrt[/var/log/app/*.log,"ERROR"]                # мониторинг логов с ротацией
```
