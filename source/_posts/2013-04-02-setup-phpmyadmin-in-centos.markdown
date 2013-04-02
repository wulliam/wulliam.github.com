---
layout: post
title: "Setup phpMyAdmin in centos"
date: 2013-04-02 10:50
comments: true
categories: Nginx phpMyAdmin Centos
---

* Check centos version

```
[root@centos5 ~]# lsb_release -a
LSB Version:	:core-4.0-amd64:core-4.0-ia32:core-4.0-noarch:graphics-4.0-amd64:graphics-4.0-ia32:graphics-4.0-noarch:printing-4.0-amd64:printing-4.0-ia32:printing-4.0-noarch
Distributor ID:	CentOS
Description:	CentOS release 5.6 (Final)
Release:	5.6
Codename:	Final
```

```
[root@centos5 ~]# uname -a
Linux centos5.localdomain 2.6.18-238.el5xen #1 SMP Thu Jan 13 16:41:45 EST 2011 x86_64 x86_64 x86_64 GNU/Linux
```

It's 64-bit. "x86_64" means 64-bit. If it was 32-bit, it would be "i686"

* Check to find nginx installed

```
[root@centos5 ~]# yum list installed | grep nginx
nginx.x86_64                          1.2.6-1.el5.ngx                  installed
```
* Need to install php
```
yum install php php-fpm php-common php-mysql
```

* Can not install php-fpm

```
[root@centos5 ~]# yum install php-fpm
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.grandcloud.cn
 * epel: mirrors.ustc.edu.cn
 * extras: mirrors.grandcloud.cn
 * updates: mirrors.grandcloud.cn
Setting up Install Process
No package php-fpm available.
Nothing to do
```
By using the remi,remi-test repository to install php-fpm.

```
[root@centos5 ~]# yum --enablerepo=remi,remi-test install php-fpm
```

* Start php-fpm and check 

```
[root@centos5 ~]# /etc/init.d/php-fpm start
Starting php-fpm:                                          [  OK  ]

```

Manually start

```
/etc/init.d/nginx start
/etc/init.d/php-fpm start
```
or

```
service nginx start
service php-fpm start
```

To start on boot

```
chkconfig --add nginx
chkconfig --levels 235 nginx on
```

```
chkconfig --add php-fpm
chkconfig --levels 235 php-fpm on
```

* Create folder for website and logs

```
[root@centos5 ~]# mkdir -p /opt/web/www/phpmyadmin/public_html
[root@centos5 ~]# mkdir -p /opt/web/www/phpmyadmin/logs
[root@centos5 ~]# chown -R nginx:nginx /opt/web/www/
```

* Configuring Nginx

Creating public folders and logs:

``
[root@centos5 ~]# mkdir -p /opt/web/www/phpmyadmin/public_html
[root@centos5 ~]# mkdir -p /opt/web/www/phpmyadmin/logs
[root@centos5 ~]# chown -R nginx:nginx /opt/web/www/
``

Create folers for virtual hosts

``
[root@centos5 ~]# mkdir /etc/nginx/sites-available
[root@centos5 ~]# mkdir /etc/nginx/sites-enabled
``

Add to /etc/nginx/nginx.conf file after "include /etc/nginx/conf.d/*.conf":``include /etc/nginx/sites-enabled/*;``

Add phpmyadmin virtual host as below:

```
server {
    server_name phpmyadmin;
    access_log /opt/web/www/phpmyadmin/logs/access.log;
    error_log /opt/web/www/phpmyadmin/logs/error.log;
    root /opt/web/www/phpmyadmin/public_html;

    location / {
        index index.html index.htm index.php;
    }

    location ~ \.php$ {
        include /etc/nginx/fastcgi_params;
        fastcgi_pass  127.0.0.1:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME /opt/web/www/phpmyadmin/public_html$fastcgi_script_name;
    }
}

```

Create a symlink to the virtual host and restart Nginx

```
[root@centos5 nginx]# ln -s /etc/nginx/sites-available/phpmyadmin /etc/nginx/sites-enabled/phpmyadmin
[root@centos5 nginx]# service nginx restart
```

Create a phpinfo.php file to test PHP-FPM

```
[root@centos5 nginx]# vi /opt/web/www/phpmyadmin/public_html/info.php


<?php
phpinfo();
?>

```

Add virtual Domain to host file


```
#vi /etc/hosts
127.0.0.1      localhost.localdomain localhost phpmyadmin

```

Now access to htp://phpmyadmin/info.php to verify phpmyadmin website works.


Optional, replace ``
fastcgi_pass 127.0.0.1:9000;
``
with
```
fastcgi_pass unix:/var/run/php5-fpm.sock;
```

* Install phpMyAdmin

```
[root@centos5 ~]# yum --enablerepo=remi,remi-test install phpmyadmin --skip-broken
```

or you find phpMyAdmin alread existed in ``/usr/share/phpMyAdmin`` 

You can make a link 

```
[root@centos5 ~]# ln -s /usr/share/phpMyAdmin /opt/web/www/phpmyadmin/public_html/phpMyAdmin
```

* Please restart php-fpm before test phpMyAdmin ``service php-fpm restart``

* If you do not want to use phpMyAdmin virtual host, you can just add a locaiton in default host.

Just edit ``# vi /etc/nginx/conf.d/default.conf `` add

```
    location ~ \.php$ {
        root /opt/web/www/phpmyadmin/public_html;
        index index.php;
        include /etc/nginx/fastcgi_params;
        fastcgi_pass  127.0.0.1:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME /opt/web/www/phpmyadmin/public_html$fastcgi_script_name;
    }
    
```

```
location /phpMyAdmin/ {
        root /opt/web/www/phpmyadmin/public_html;
        index  index.html index.htm;
            location ~ \.php$ {
                fastcgi_index index.php;
                include /etc/nginx/fastcgi_params;
                fastcgi_pass  127.0.0.1:9000;
                fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                #fastcgi_param SCRIPT_FILENAME /opt/web/www/phpmyadmin/public_html/$fastcgi_script_name;
            }
    }
```