# 설치

### nginx 설치

``` vim
[root@ip-172-31-31-206 sman]# yum info nginx
Loaded plugins: fastestmirror, presto
Loading mirror speeds from cached hostfile
 * base: ftp.riken.jp
 * extras: ftp.riken.jp
 * updates: ftp.riken.jp
Available Packages
Name        : nginx
Arch        : x86_64
Version     : 1.8.0
Release     : 1.el6.ngx
Size        : 352 k
Repo        : nginx
Summary     : High performance web server
URL         : http://nginx.org/
License     : 2-clause BSD-like license
Description : nginx [engine x] is an HTTP and reverse proxy server, as well as
            : a mail proxy server.

[root@ip-172-31-31-206 sman]# yum install nginx

```

### php 설치

yum repository 추가.

``` vim
# rpm -Uvh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
# rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
# yum repolist

```
설치 할 파일 확인.

``` vim
[root@ip-172-31-31-206 sman]# yum --enablerepo=remi-php55,remi list php php-mbstring php-mcrypt php-fpm php-pdo php-mysqlnd php-pecl-memcached php-pecl-redis php-opcache
Loaded plugins: fastestmirror, presto
Loading mirror speeds from cached hostfile
 * base: ftp.riken.jp
 * epel: s3-mirror-ap-northeast-1.fedoraproject.org
 * extras: ftp.riken.jp
 * remi: remi.kazukioishi.net
 * remi-php55: remi.kazukioishi.net
 * updates: ftp.riken.jp
remi                                                     | 2.9 kB     00:00
remi/primary_db                                          | 1.1 MB     00:00
remi-php55                                               | 2.9 kB     00:00
remi-php55/primary_db                                    | 185 kB     00:00
Available Packages
php.x86_64                           5.5.27-1.el6.remi                remi-php55
php-fpm.x86_64                       5.5.27-1.el6.remi                remi-php55
php-mbstring.x86_64                  5.5.27-1.el6.remi                remi-php55
php-mcrypt.x86_64                    5.5.27-1.el6.remi                remi-php55
php-mysqlnd.x86_64                   5.5.27-1.el6.remi                remi-php55
php-opcache.x86_64                   5.5.27-1.el6.remi                remi-php55
php-pdo.x86_64                       5.5.27-1.el6.remi                remi-php55
php-pecl-memcached.x86_64            2.2.0-2.el6.remi.5.5             remi-php55
php-pecl-redis.x86_64                2.2.7-1.el6.remi.5.5             remi-php55
[root@ip-172-31-31-206 sman]#

```

설치

``` vim
# yum --enablerepo=remi-php55,remi install php php-mbstring php-mcrypt php-fpm php-pdo php-mysqlnd php-pecl-memcached php-pecl-redis php-opcache

```

user, group 을 nginx 로 변경을 한다.

``` vim
# vi /etc/php-fpm.d/www.conf

; Unix user/group of processes
; Note: The user is mandatory. If the group is not set, the default user's group
;       will be used.
; RPM: apache Choosed to be able to access some dir as httpd
user = nginx
; RPM: Keep a group allowed to write in log dir.
group = nginx

```

php-fpm 을 시작한다.

``` vim
[root@ip-172-31-31-206 sman]# service php-fpm start
Starting php-fpm:                                          [  OK  ]
[root@ip-172-31-31-206 sman]#

```

nginx 의 default.conf 를 수정을 해서 php 가 정상적으로 작동이 되는지 확인 한다.
``` php
[root@ip-172-31-31-206 sman]# vi /etc/nginx/conf.d/default.conf
[root@ip-172-31-31-206 sman]# service nginx restart

# vi /usr/share/nginx/html/index.php
<?php
  echo __DIR__;
  phpinfo();
?>

```

### Installation phalcon

tool 설치.
``` vim
[root@ip-172-31-31-206 sman]# yum --enablerepo=remi-php55,remi list php-devel pcre-devel.x86_64 gcc
Loaded plugins: fastestmirror, presto
Loading mirror speeds from cached hostfile
 * base: ftp.riken.jp
 * epel: s3-mirror-ap-northeast-1.fedoraproject.org
 * extras: ftp.riken.jp
 * remi: remi.kazukioishi.net
 * remi-php55: remi.kazukioishi.net
 * updates: ftp.riken.jp
Available Packages
gcc.x86_64                         4.4.7-11.el6                       base
pcre-devel.x86_64                  7.8-6.el6                          base
php-devel.x86_64                   5.5.27-1.el6.remi                  remi-php55
[root@ip-172-31-31-206 sman]# yum --enablerepo=remi-php55,remi install php-devel pcre-devel.x86_64 gcc
```

``` vim
[root@ip-172-31-31-206 sman]# yum install git
Loaded plugins: fastestmirror, presto
Setting up Install Process
Loading mirror speeds from cached hostfile
 * base: ftp.riken.jp
 * epel: s3-mirror-ap-northeast-1.fedoraproject.org
 * extras: ftp.riken.jp
 * updates: ftp.riken.jp
Resolving Dependencies
--> Running transaction check
---> Package git.x86_64 0:1.7.1-3.el6_4.1 will be installed
--> Processing Dependency: perl-Git = 1.7.1-3.el6_4.1 for package: git-1.7.1-3.el6_4.1.x86_64
--> Processing Dependency: perl(Git) for package: git-1.7.1-3.el6_4.1.x86_64
--> Processing Dependency: perl(Error) for package: git-1.7.1-3.el6_4.1.x86_64
--> Running transaction check
---> Package perl-Error.noarch 1:0.17015-4.el6 will be installed
---> Package perl-Git.noarch 0:1.7.1-3.el6_4.1 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

================================================================================
 Package            Arch           Version                   Repository    Size
================================================================================
Installing:
 git                x86_64         1.7.1-3.el6_4.1           base         4.6 M
Installing for dependencies:
 perl-Error         noarch         1:0.17015-4.el6           base          29 k
 perl-Git           noarch         1.7.1-3.el6_4.1           base          28 k

Transaction Summary
================================================================================
Install       3 Package(s)

Total download size: 4.7 M
Installed size: 15 M
Is this ok [y/N]: y

```

소스 다운 받아서 install
``` vim
[sman@ip-172-31-31-206 ~]$ git clone git://github.com/phalcon/cphalcon.git
Initialized empty Git repository in /home/sman/cphalcon/.git/
remote: Counting objects: 91357, done.
remote: Compressing objects: 100% (85/85), done.
remote: Total 91357 (delta 24), reused 0 (delta 0), pack-reused 91272
Receiving objects: 100% (91357/91357), 82.06 MiB | 6.35 MiB/s, done.
Resolving deltas: 100% (70707/70707), done.
[sman@ip-172-31-31-206 ~]$
[sman@ip-172-31-31-206 ~]$ ls
cphalcon
[sman@ip-172-31-31-206 ~]$ cd cphalcon/build/
[sman@ip-172-31-31-206 build]$ su
Password:
[root@ip-172-31-31-206 build]# ./install

See any operating system documentation about shared libraries for
more information, such as the ld(1) and ld.so(8) manual pages.
----------------------------------------------------------------------

Build complete.
Don't forget to run 'make test'.

Installing shared extensions:     /usr/lib64/php/modules/
Installing header files:          /usr/include/php/

Thanks for compiling Phalcon!
Build succeed: Please restart your web server to complete the installation
[root@ip-172-31-31-206 build]#


```

phalcon.ini 파일에 extension=phalcon.so 추가 한다.
``` vim
[root@ip-172-31-31-206 sman]# vi /etc/php.d/phalcon.ini

[root@ip-172-31-31-206 sman]# service php-fpm restart
Stopping php-fpm:                                          [  OK  ]
Starting php-fpm:                                          [  OK  ]
[root@ip-172-31-31-206 sman]#
```

![haroopad icon](https://lh3.googleusercontent.com/B2CHbjNzbghHaqCOhtpjLMX_g0AHZMZmmQ-n0Lrolg=w617-h514-no)


``` bash
[sman@ip-172-31-31-206 ~]$ curl -s http://getcomposer.org/installer | php
#!/usr/bin/env php
All settings correct for using Composer
Downloading...

Composer successfully installed to: /home/sman/composer.phar
Use it: php composer.phar
[sman@ip-172-31-31-206 ~]$ ls
composer.phar  cphalcon

[sman@ip-172-31-31-206 ~]$ vi composer.json
{
    "require": {
        "phalcon/devtools": "dev-master"
    }
}

[sman@ip-172-31-31-206 ~]$ php composer.phar install

[root@ip-172-31-31-206 sman]# ln -s /home/sman/vendor/phalcon/devtools/phalcon.php /usr/bin/phalcon
[root@ip-172-31-31-206 sman]# chmod ugo+x /usr/bin/phalcon
[root@ip-172-31-31-206 sman]# phalcon commands

Phalcon DevTools (2.0.5)

Available commands:
  commands (alias of: list, enumerate)
  controller (alias of: create-controller)
  model (alias of: create-model)
  all-models (alias of: create-all-models)
  project (alias of: create-project)
  scaffold (alias of: create-scaffold)
  migration (alias of: create-migration)
  webtools (alias of: create-webtools)

[root@ip-172-31-31-206 sman]#

# cd /usr/share/nginx/html
# phalcon create-project store
# chown -R nginx:nginx /usr/share/nginx/html/store/app/cache


```


``` vim
server {
    listen   80;
    server_name localhost.dev;

    index index.php index.html index.htm;
    set $root_path '/usr/share/nginx/html/store/public';
    root $root_path;

    location / {
        try_files $uri $uri/ /index.php;
    }

    location ~ \.php$ {
            try_files $uri =404;
            fastcgi_split_path_info ^(.+\.php)(/.+)$;
            fastcgi_pass 127.0.0.1:9000;
            fastcgi_index index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include fastcgi_params;
    }

    location ~* ^/(css|img|js|flv|swf|download)/(.+)$ {
        root $root_path;
    }

    location ~ /\.ht {
        deny all;
    }
}

```

### memcached 설치.
``` bash
[root@ip-172-31-31-206 public]# yum --enablerepo=remi install memcached

================================================================================
 Package           Arch           Version                    Repository    Size
================================================================================
Installing:
 memcached         x86_64         1.4.22-1.el6.remi          remi          89 k

Transaction Summary
================================================================================
Install       1 Package(s)


```

### redis 설치.

``` bash
[root@ip-172-31-31-206 public]# yum --enablerepo=remi install redis
================================================================================
 Package          Arch           Version                     Repository    Size
================================================================================
Installing:
 redis            x86_64         3.0.2-1.el6.remi            remi         430 k
Installing for dependencies:
 jemalloc         x86_64         3.6.0-1.el6                 epel         100 k

Transaction Summary
================================================================================
Install       2 Package(s)


```


### mariadb 설치.

``` bash
[root@ip-172-31-31-206 public]# vi /etc/yum.repos.d/MariaDB.repo
# MariaDB 5.5 CentOS repository list - created 2015-06-25 10:44 UTC
# http://mariadb.org/mariadb/repositories/
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/5.5/centos6-amd64
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1


[root@ip-172-31-31-206 public]# yum list MariaDB-server MariaDB-client
Loaded plugins: fastestmirror, presto
Loading mirror speeds from cached hostfile
 * base: ftp.riken.jp
 * epel: s3-mirror-ap-northeast-1.fedoraproject.org
 * extras: ftp.riken.jp
 * updates: ftp.riken.jp
mariadb                                                  | 2.9 kB     00:00
mariadb/primary_db                                       |  20 kB     00:00
Available Packages
MariaDB-client.x86_64                    5.5.44-1.el6                    mariadb
MariaDB-server.x86_64                    5.5.44-1.el6                    mariadb
[root@ip-172-31-31-206 public]#

[root@ip-172-31-31-206 public]# yum install MariaDB-server MariaDB-client


```






















