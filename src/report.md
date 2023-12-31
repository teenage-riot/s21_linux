## Part 1. Установка ОС
+ Ввожу имя пользователя:  
![](img/part_1/installing_1.png)
+ Версия системы:  
![](img/part_1/installing_2.png)

## Part 2. Создание пользователя
+ Команда для создания пользователя и её вывод:  
![](img/part_2/new_user_1.png)
+ cat /etc/passwd:  
![](img/part_2/new_user_2.png)

## Part 3. Настройка сети ОС
+ Устанавливаю название машины как user-1, отредактировав файлы /etc/hostname и /etc/hosts:  
![](img/part_3/hosts.png)
+ Вывод команды hostname после перезагрузки:  
![](img/part_3/new_hostname.png)
+ Устанавливаю временную зону, соответствующую нашему текущему местоположению. В Казани одно время с Москвой.
![](img/part_3/timezone.png)
+ Вывожу названия сетевых интерфейсов с помощью консольной команды.
![](img/part_3/ip_link.png)
(Интерфейс **lo** присутствует потому, что это т.н. loopback - способ связи компьютера с самим собой. Доступ к компьютеру через знаменитое 127.0.0.1 (a.k.a. localhost) возможен благодаря ему.)  
+ Используя консольную команду, получаю ip адрес устройства, на котором я работаю, от DHCP сервера.  
![](img/part_3/ip_a.png)
(DHCP расшифровывается как Dynamic Host Configuration Protocol — протокол динамической настройки узла.)  
+ Узнаю внешний ip-адрес шлюза (ip)...  
![](img/part_3/ip.png)
+ ...и внутренний, он же ip-адрес по умолчанию (gw).
![](img/part_3/gw.png)
+ Задаю статичные настройки, отредактировав файл в директории /etc/netplan/
![](img/part_3/static.png)
+ После перезагрузки:
![](img/part_3/reboot.png)
+ Пингую удаленные хосты 1.1.1.1 и ya.ru:
![](img/part_3/ping.png)

## Part 4. Обновление ОС
+ В результате команды do-release-upgrade -d произошло обновление.
![](img/part_4/upgrade.png)

## Part 5. Использование команды sudo
 Команда **sudo** ( **s**ubstitute **u**ser and **do**, подменить пользователя и выполнить) позволяет строго определенным пользователям выполнять указанные программы с административными привилегиями без ввода пароля суперпользователя **root**. Если быть точнее, то команда sudo позволяет выполнять программы от имени любого пользователя, но, если идентификатор или имя этого пользователя не указаны, то предполагается выполнение от имени суперпользователя root. Таким образом, использование sudo позволяет выполнять привилегированные команды обычным пользователям без необходимости ввода пароля суперпользователя root .
+ Разрешаю пользователю, созданному в Part 2, выполнять команду sudo.
![](img/part_5/sudo.png)
+ Меняю hostname ОС от его иммени.
![](img/part_5/host.png)

## Part 6. Установка и настройка службы времени
![](img/part_6/time.png)

## Part 7. Установка и использование текстовых редакторов
### Создание файла
+ Vim
![](img/part_7/vim1.png)
Для выхода c сохранением изменений я нажал Esc, затем ввёл :x и нажал Enter.
+ Nano
![](img/part_7/nano1.png)
Для выхода c сохранением изменений я нажал Ctrl+O, затем Enter, затем Ctrl+X.
+ Joe
![](img/part_7/joe1.png)
Для выхода c сохранением изменений я нажал Ctrl+K, затем X.
### Редактирование файла
+ Vim
![](img/part_7/vim2.png)
Для выхода без сохранения я нажал Esc, затем ввёл :q! и нажал Enter.
+ Nano
![](img/part_7/nano2.png)
Для выхода c сохранением изменений я нажал Ctrl+X, затем N.
+ Joe
![](img/part_7/joe2.png)
Для выхода c сохранением изменений я нажал Ctrl+C, затем Y.
### Поиск и замена внутри файла
+ Vim - поиск
![](img/part_7/vim3.png)
+ Vim - замена
![](img/part_7/vim4.png)
+ Nano - поиск
![](img/part_7/nano3.png)
+ Nano - замена
![](img/part_7/nano4.png)
+ Joe - поиск
![](img/part_7/joe3.png)
+ Joe - замена
![](img/part_7/joe4.png)

## Part 8. Установка и базовая настройка сервиса SSHD
+ Установка службы SSHd.  
![](img/part_8/sshd_ins.png)
+ Добавление автостарта службы при загрузке системы.  
Для этого используем команду systemctl enable sshd.service.
![](img/part_8/sshd_en.png)
+ Перенастраиваем службу SSHd на порт 2022.
vim /etc/ssh/sshd_config
![](img/part_8/sshd_2022.png)
/etc/init.d/ssh restart
+ Наличие процесса sshd:
![](img/part_8/sshd_proc.png)
Ключ -е используется для показа всех процессов со стандартным синтаксисом, а grep достаёт из списка нужную нам строку.
+ Вывод команды netstat -tan:
![](img/part_8/netstat.png)
Ключ -n показывает номерные адреса вместо того, чтобы пытаться определить символические.  
Ключ -а показывает все сокеты: и прослушиваемые, и нет.  
Ключ -t показывает TCP-порты.  
  
**Значение каждого столбца**  
Proto: протокол, используемый сокетом.  
Recv-Q: количество байтов, не скопированных пользовательской программой, подключенной к этому сокету.  
Send-Q: количество неподтвержденных байтов удаленного хоста  
Local Address: локальный адрес (имя локального хоста) и номер порта сокета. Если не указана опция -n, адрес сокета разрешается в соответствии с полным именем хоста (FQDN), а номер порта преобразуется в соответствующее имя службы.  
Foreign Address: удаленный адрес (имя удаленного хоста) и номер порта сокета.  
State: состояние сокета. LISTEN означает ожидание входящих соединений.  
  
**0.0.0.0** в колонке значит, что на данном порту слушаются все сетевые интерфейсы.

## Part 9. Установка и использование утилит top, htop
+ Вывод команды top.
![](img/part_9/top.png)
  
uptime: 48 минут  
количество авторизованных пользователей: 1  
общая загрузка системы за последнюю минуту, 5 и 15 минут: 0, 0, 0  
общее количество процессов: 96  
загрузка cpu: 0, простаивающая вычислительная мощность - 100  
загрузка памяти: всего 976.9, 159.1 свободно, 155.6 использовано, 662.2 задействовано в буфере и кэше  
pid процесса занимающего больше всего памяти: 1  
pid процесса, занимающего больше всего процессорного времени: 2528

**htop**
+ отсортированo по PID
![](img/part_9/htop1.png)
+ по PERCENT_CPU 
![](img/part_9/htop2.png)
+ по PERCENT_MEM
![](img/part_9/htop3.png)
+ по TIME
![](img/part_9/htop4.png)
+ отфильтровано для процесса sshd
![](img/part_9/htop5.png)
+ с процессом syslog, найденным, используя поиск
![](img/part_9/htop6.png)
+ с добавленным выводом hostname, clock и uptime
![](img/part_9/htop7.png)

## Part 10. Использование утилиты fdisk
![](img/part_10/fdisk.png)
  
Жёсткий диск называется /dev/sda, его размер 10 гб, количество секторов 20967424 (2048+1857596+19107840).

+ Под swap выделено 1.7 гб:  
![](img/part_10/swap.png)  

## Part 11. Использование утилиты df
+ Команда df.
![](img/part_11/df.png)  
  
Размер корневого размера 9336140 кб, занято 4364608 кб, свободно 4477556 кб, использовано 50%.

+ Команда df -Th.
![](img/part_11/df-th.png)  
  
Размер корневого размера 9.0 гб, занято 4.2 гб, свободно 4.3 гб, использовано 50%. Файловая система ext4.

## Part 12. Использование утилиты du
+ Вывод размера папок /home, /var, /var/log (в байтах и в человекочитаемом виде):
![](img/part_12/du_size.png)  
+ Вывод размера всего содержимого в /var/log
![](img/part_12/du_var.png)  

## Part 13. Установка и использование утилиты ncdu
![](img/part_13/home.png)  
![](img/part_13/var.png)  
![](img/part_13/var_log.png)  

## Part 14. Работа с системными журналами
+ Часть вывода /var/log/dmesg
![](img/part_14/dmesg.png)  
+ Часть вывода /var/log/syslog
![](img/part_14/syslog.png)  
+ Часть вывода /var/log/auth.log
![](img/part_14/auth.png)  

Время последней успешной авторизации - 15:45:26, пользователь libbieev, метод входа - залогинился.

+ Перезапускаем sshd и убеждаемся в её рестарте
![](img/part_14/sshd.png)  

## Part 15. Использование планировщика заданий CRON

+ crontab -e  
![](img/part_15/crontab-e.png)  
+ строчки из системного журнала:  
![](img/part_15/cron_up.png)  
+ удаление заданий:  
![](img/part_15/cron-r-i.png)  
  