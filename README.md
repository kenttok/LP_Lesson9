Часть 1. Пишем свой сервис
Результат:

tail -f /var/log/messages

Jun 7 11:39:37 sysd-less9 systemd: Starting My watchlog service...
Jun 7 11:39:37 sysd-less9 root: Jun 7 11:39:17 UTC 2023: I found word, Master!
Jun 7 11:39:37 sysd-less9 systemd: Started My watchlog service.
Jun 7 11:40:08 sysd-less9 systemd: Starting My watchlog service...
Jun 7 11:40:08 sysd-less9 root: Jun 7 11:40:08 UTC 2023: I found word, Master!
Jun 7 11:40:08 sysd-less9 systemd: Started My watchlog service.
Jun 7 11:40:38 sysd-less9 systemd: Starting My watchlog service...
Jun 7 11:40:38 sysd-less9 root: Jun 7 11:41:08 UTC 2023: I found word, Master!
Jun 7 11:40:38 sysd-less9 systemd: Started My watchlog service.
Jun 7 11:41:08 sysd-less9 systemd: Starting My watchlog service...
Jun 7 11:41:08 sysd-less9 root: Jun 7 11:41:48 UTC 2023: I found word, Master!
Jun 7 11:41:08 sysd-less9 systemd: Started My watchlog service.

Часть 2. Из epel установить spawn-fcgi и переписать init-скрипт на unit-файл.

Результат:
systemctl status spawn-fcgi
● spawn-fcgi.service - Spawn-fcgi startup service by Otus
   Loaded: loaded (/etc/systemd/system/spawn-fcgi.service; disabled; vendor preset: disabled)
   Active: active (running)
 Main PID: 2542 (php-cgi)
   CGroup: /system.slice/spawn-fcgi.service
           ├─2542 /usr/bin/php-cgi
           ├─2543 /usr/bin/php-cgi
           ├─2544 /usr/bin/php-cgi
           ├─2545 /usr/bin/php-cgi
           ├─2546 /usr/bin/php-cgi
           ├─2547 /usr/bin/php-cgi
           ├─2548 /usr/bin/php-cgi
           ├─2549 /usr/bin/php-cgi
           ├─2550 /usr/bin/php-cgi
           ├─2551 /usr/bin/php-cgi
           ├─2552 /usr/bin/php-cgi
           ├─2553 /usr/bin/php-cgi
           ├─2554 /usr/bin/php-cgi
           ├─2555 /usr/bin/php-cgi
           ├─2556 /usr/bin/php-cgi
           ├─2557 /usr/bin/php-cgi
           ├─2558 /usr/bin/php-cgi
           ├─2559 /usr/bin/php-cgi
           ├─2560 /usr/bin/php-cgi
           ├─2561 /usr/bin/php-cgi
           ├─2562 /usr/bin/php-cgi
           ├─2563 /usr/bin/php-cgi
           ├─2564 /usr/bin/php-cgi
           ├─2565 /usr/bin/php-cgi
           ├─2566 /usr/bin/php-cgi
           ├─2567 /usr/bin/php-cgi
           ├─2568 /usr/bin/php-cgi
           ├─2569 /usr/bin/php-cgi
           ├─2570 /usr/bin/php-cgi
           ├─2571 /usr/bin/php-cgi
           ├─2572 /usr/bin/php-cgi
           ├─2573 /usr/bin/php-cgi
           └─2574 /usr/bin/php-cgi

Часть 3. Дополнить юнит-файл apache httpd возможностью запустить несколько инстансов сервера с разными конфигами

Результат, httpd слушает два порта.

[root@lvm ~]# ss -tnulp | grep httpd
tcp    LISTEN     0      128      :::8080                 :::*                   users:(("httpd",pid=6212,fd=4),("httpd",pid=6211,fd=4),("httpd",pid=6210,fd=4),("httpd",pid=6209,fd=4),("httpd",pid=6208,fd=4),("httpd",pid=6207,fd=4))
tcp    LISTEN     0      128      :::80                   :::*                   users:(("httpd",pid=6200,fd=4),("httpd",pid=6199,fd=4),("httpd",pid=6198,fd=4),("httpd",pid=6197,fd=4),("httpd",pid=6196,fd=4),("httpd",pid=6195,fd=4))
[root@lvm ~]# 
