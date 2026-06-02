# 🪟 Полный список команд PowerShell (шпаргалка)

## 🔧 Основные команды для навигации
```
Get-Location                             # показать текущую директорию (pwd)
Set-Location C:\path                     # перейти в директорию (cd)
Set-Location ..                          # перейти на уровень вверх
Set-Location ~                           # перейти в домашнюю папку
Get-ChildItem                            # список файлов и папок (ls, dir)
Get-ChildItem -Recurse                   # рекурсивный список всех файлов/папок
Get-ChildItem -Force                     # показать скрытые файлы
Get-ChildItem -File                      # показать только файлы
Get-ChildItem -Directory                 # показать только папки
Get-ChildItem -Recurse -Filter "*.txt"   # найти все .txt файлы
Get-ChildItem -Recurse -Name             # только имена файлов (без информации)
Get-ChildItem | Format-Table -AutoSize   # отформатировать вывод в таблицу
```

## 📁 Работа с файлами и папками
```
New-Item -Path . -Name "file.txt" -ItemType File    # создать файл
New-Item -Path "C:\folder" -ItemType Directory      # создать папку
New-Item -Path "C:\folder\subfolder" -ItemType Directory -Force  # создать с вложенными
Copy-Item "source.txt" "dest.txt"                   # скопировать файл (cp)
Copy-Item "C:\source" "C:\dest" -Recurse            # рекурсивно скопировать папку
Move-Item "old.txt" "new.txt"                       # переименовать/переместить (mv)
Remove-Item "file.txt"                              # удалить файл (rm, del)
Remove-Item "C:\folder" -Recurse                    # удалить папку со всем содержимым
Remove-Item "*.tmp" -Force                          # принудительно удалить (без запросов)
Clear-Content "file.txt"                            # очистить содержимое файла
Get-Content "file.txt"                              # показать содержимое файла (cat)
Get-Content "file.txt" -Tail 20                     # последние 20 строк (tail -20)
Get-Content "file.txt" -Head 20                     # первые 20 строк (head -20)
Get-Content "file.txt" -Wait                        # следить за файлом в реальном времени (tail -f)
Get-Content "file.txt" -TotalCount 50               # первые 50 строк
Add-Content "file.txt" "new line"                   # добавить строку в конец файла
Set-Content "file.txt" "overwrite"                  # перезаписать содержимое файла
```

## 📊 Информация о системе
```
Get-ComputerInfo                                   # подробная информация о компьютере
Get-ComputerInfo -Property WindowsVersion          # только версия Windows
Get-ComputerInfo -Property CsProcessors            # информация о процессорах
Get-ComputerInfo -Property CsTotalPhysicalMemory   # общий объём RAM
Get-WmiObject -Class Win32_OperatingSystem         # информация об ОС
Get-WmiObject -Class Win32_ComputerSystem          # информация о компьютере
Get-WmiObject -Class Win32_BIOS                    # информация о BIOS
Get-WmiObject -Class Win32_LogicalDisk             # информация о дисках
Get-WmiObject -Class Win32_Processor               # информация о процессоре
Get-WmiObject -Class Win32_PhysicalMemory          # информация о планках RAM
Get-CimInstance Win32_OperatingSystem              # альтернатива WMI (CIM)
Get-CimInstance Win32_ComputerSystem
Get-CimInstance Win32_LogicalDisk
Get-CimInstance Win32_Processor
Get-CimInstance Win32_PhysicalMemory
Get-Process                                        # список процессов (ps)
Get-Process | Sort-Object -Property CPU -Descending # процессы по убыванию CPU
Get-Process | Where-Object {$_.CPU -gt 100}        # процессы с CPU > 100
Stop-Process -Name "notepad"                       # остановить процесс по имени (kill)
Stop-Process -Id 1234                              # остановить процесс по PID
Get-Service                                        # список служб
Get-Service | Where-Object {$_.Status -eq "Running"} # только запущенные службы
Start-Service -Name "Spooler"                      # запустить службу
Stop-Service -Name "Spooler"                       # остановить службу
Restart-Service -Name "Spooler"                    # перезапустить службу
Get-EventLog -LogName System -Newest 20            # последние 20 событий системного лога
Get-EventLog -LogName Security -EntryType Error    # ошибки из лога безопасности
Get-WinEvent -FilterHashtable @{LogName='System'; Level=1; StartTime=(Get-Date).AddDays(-1)}  # события за последние 24 часа
```

## 💾 Диски и память
```
Get-PSDrive                                       # список дисков
Get-PSDrive -PSProvider FileSystem                # только файловые системы
Get-WmiObject -Class Win32_LogicalDisk | Select-Object DeviceID, Size, FreeSpace  # информация о дисках
Get-CimInstance Win32_LogicalDisk | Select-Object DeviceID, @{Name="SizeGB";Expression={[math]::Round($_.Size/1GB,2)}}, @{Name="FreeGB";Expression={[math]::Round($_.FreeSpace/1GB,2)}}  # размер дисков в ГБ
Get-WmiObject -Class Win32_OperatingSystem | Select-Object TotalVisibleMemorySize, FreePhysicalMemory  # состояние RAM
Get-Volume                                        # список томов (в PowerShell 6+)
Get-Partition                                     # список разделов
Get-Disk                                          # список физических дисков
```
## 🌐 Сеть
```
Get-NetIPAddress                                 # IP-адреса интерфейсов
Get-NetIPConfiguration                           # конфигурация сети (интерфейсы, DNS, маршруты)
Get-NetRoute                                     # таблица маршрутизации
Get-NetAdapter                                   # список сетевых адаптеров
Get-NetAdapter | Where-Object {$_.Status -eq "Up"} # только активные адаптеры
Get-NetTCPConnection                             # список TCP соединений
Get-NetTCPConnection | Where-Object {$_.State -eq "Listen"}  # слушающие порты
Get-NetTCPConnection -LocalPort 80               # соединения на порту 80
Get-NetUDPEndpoint                               # список UDP соединений
Test-Connection google.com                       # ping (4 пакета)
Test-Connection google.com -Count 1              # один ping
Test-Connection google.com -Continuous           # бесконечный ping (Ctrl+C остановить)
Test-NetConnection google.com -Port 80           # проверить доступность порта (telnet)
Resolve-DnsName google.com                       # DNS-запрос (nslookup)
Resolve-DnsName google.com -Type MX              # MX запись
Invoke-WebRequest -Uri http://localhost          # HTTP запрос (curl)
Invoke-WebRequest -Uri http://localhost -Method POST -Body "data"  # POST запрос
Invoke-RestMethod -Uri https://api.github.com    # запрос к REST API (JSON вывод)
```

## 👥 Пользователи и права
```
Get-LocalUser                                    # список локальных пользователей
Get-LocalUser -Name "Administrator"              # информация о конкретном пользователе
Get-LocalGroup                                   # список локальных групп
Get-LocalGroupMember -Group "Administrators"     # члены группы Administrators
New-LocalUser -Name "NewUser" -Password (ConvertTo-SecureString "P@ssw0rd" -AsPlainText -Force) -FullName "New User"  # создать пользователя
New-LocalGroup -Name "NewGroup"                  # создать группу
Add-LocalGroupMember -Group "Administrators" -Member "NewUser"  # добавить пользователя в группу
Remove-LocalUser -Name "NewUser"                 # удалить пользователя
Remove-LocalGroup -Name "NewGroup"               # удалить группу
Get-ACL "C:\folder"                              # посмотреть права доступа к папке
Set-ACL "C:\folder" -AclObject $newAcl           # установить права доступа
Get-WmiObject -Class Win32_UserAccount           # все учётные записи (включая доменные)
```

## 🔧 Процессы и службы
```
Get-Process                                      # все процессы
Get-Process -Name "explorer"                     # процесс по имени
Get-Process -Id 1234                             # процесс по PID
Stop-Process -Name "notepad"                     # остановить процесс
Stop-Process -Id 1234 -Force                     # принудительно остановить
Start-Process "notepad.exe"                      # запустить процесс
Start-Process "notepad.exe" -Wait                # запустить и ждать завершения
Get-Service                                      # все службы
Get-Service -Name "spooler"                      # служба по имени
Start-Service -Name "spooler"                    # запустить службу
Stop-Service -Name "spooler"                     # остановить службу
Restart-Service -Name "spooler"                  # перезапустить службу
Set-Service -Name "spooler" -StartupType Automatic # изменить тип запуска
```

## 📋 Работа с переменными и выводом
```
$var = "Hello"                                   # создать переменную
$var                                             # показать переменную
$var = 1..10                                     # создать массив 1-10
$var = @(1,2,3,4,5)                              # явное создание массива
$hash = @{Name="John"; Age=30}                   # создать хеш-таблицу
$hash.Name                                       # обращение по ключу
Write-Host "Hello World"                         # вывод текста (с цветом)
Write-Output "Hello World"                       # вывод текста (стандартный)
Write-Error "Error message"                      # вывод ошибки
Write-Warning "Warning message"                  # вывод предупреждения
Write-Debug "Debug message"                      # вывод отладочной информации
Write-Verbose "Verbose message"                  # подробный вывод
```

## 🔍 Фильтрация и сортировка
```
Get-Process | Where-Object {$_.CPU -gt 10}       # фильтр по CPU > 10
Get-Process | Where-Object {$_.Name -like "*win*"}  # фильтр по имени (like)
Get-Service | Where-Object {$_.Status -eq "Running" -and $_.Name -like "*sql*"}  # несколько условий
Get-Process | Sort-Object -Property CPU -Descending # сортировка по убыванию CPU
Get-Process | Select-Object -First 10            # первые 10 процессов
Get-Process | Select-Object Name, CPU, PM        # выбрать только нужные поля
Get-Process | Select-Object -Unique              # убрать дубликаты
Get-ChildItem | Where-Object {$_.Length -gt 1MB} # файлы больше 1 МБ
Get-ChildItem -Recurse | Group-Object Extension | Sort-Object Count -Descending # сгруппировать по расширениям
```

## 🔄 Конвейеры (Pipeline)
```
Get-Process | Stop-Process -WhatIf               # показать, что будет без выполнения
Get-Process | Stop-Process -Confirm              # запросить подтверждение
Get-ChildItem *.log | Remove-Item -Verbose       # удалить с подробным выводом
Get-Service | Export-Csv services.csv            # экспортировать в CSV
Get-Process | ConvertTo-Json | Out-File processes.json  # экспортировать в JSON
Import-Csv file.csv | ForEach-Object {Write-Host $_.Name}  # импортировать CSV
Get-ChildItem | ForEach-Object { $_.FullName }   # для каждого элемента сделать действие
1..10 | ForEach-Object { $_ * $_ }               # вычислить квадраты чисел от 1 до 10
```
## 📝 Работа с текстом
```
"Hello World".ToUpper()                         # преобразовать в верхний регистр
"Hello World".ToLower()                         # преобразовать в нижний регистр
"Hello World".Replace("World", "PowerShell")    # замена текста
"Hello World".Contains("World")                 # проверка наличия подстроки
"Hello World".Split(" ")                        # разделить строку в массив
"Hello" + " " + "World"                         # конкатенация строк
"Hello {0} {1}" -f "PowerShell", "World"        # форматирование строки
Get-Content file.txt | Select-String "error"    # найти строки с "error" (grep)
Get-Content file.txt | Select-String "error" -CaseSensitive  # с учётом регистра
Get-Content file.txt | Measure-Object -Line     # количество строк (wc -l)
```

## 📦 Управление модулями
```
Get-Module -ListAvailable                       # список установленных модулей
Get-Module                                      # список загруженных модулей
Find-Module -Name "Az"                          # найти модуль в репозитории
Install-Module -Name "Az" -Force                # установить модуль
Update-Module -Name "Az"                        # обновить модуль
Remove-Module -Name "Az"                        # выгрузить модуль
Uninstall-Module -Name "Az"                     # удалить модуль
Import-Module -Name "ActiveDirectory"           # загрузить модуль
```
## 🏗️ Управление задачами (Jobs)
```
Start-Job -ScriptBlock {Get-Process}            # запустить фоновую задачу
Start-Job -Name "MyJob" -ScriptBlock {Get-Service}  # с именем
Get-Job                                         # список фоновых задач
Get-Job -Name "MyJob"                           # получить конкретную задачу
Receive-Job -Id 1                               # получить результат задачи
Receive-Job -Id 1 -Wait                         # ждать выполнения
Stop-Job -Id 1                                  # остановить задачу
Remove-Job -Id 1                                # удалить задачу
```
## 🔐 Администрирование с правами администратора
```
Start-Process powershell -Verb RunAs            # открыть PowerShell от администратора
Start-Process powershell -Verb RunAs -ArgumentList "-Command Get-Service"  # выполнить команду от администратора
Start-Process -FilePath "cmd.exe" -Verb RunAs   # открыть CMD от администратора
# Запуск PowerShell от администратора в Windows:
# ПКМ по иконке PowerShell → "Run as Administrator"
```

## ✅ Что означают нормальные значения
```
Показатель                 Норма          Проблема
$? (последняя команда)     True           False (ошибка)
Get-Process                ожидаемые      слишком много процессов (утечка)
Get-Service                Running        Stopped (критичные службы)
Test-Connection            Success        Timeout, Destination unreachable
Get-EventLog               ожидаемые      много ошибок (WARNING, ERROR)
Get-WmiObject              данные получены Access denied, Not found
свободное место (Get-PSDrive) > 20%       < 10% (диск полон)
```
Основано на официальной документации Microsoft PowerShell.
Для более детального изучения: Get-Help command
или официальная документация: https://docs.microsoft.com/en-us/powershell
