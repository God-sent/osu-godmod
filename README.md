# osu! godmod private server
osu! private server dev repository.
Location - RU

Репозиторий состоит из библиотек и форков разного рода API, микро-инструкций и списка дел.
Дневник разработки также здесь.


Необходимы:
- nginx
- PHP
- mySQL
- Python 3
- flask
- tornado
- bcrypt 
- psutils
- pymySQL
- Форки репозитория /Ripple/

Репозитории:
```
git clone --recursive https://github.com/semyon422/open-ripple
git clone --recursive https://github.com/semyon422/omppc
git clone --recursive https://github.com/osufx/pep.py
git clone --recursive https://github.com/osufx/lets
git clone --recursive https://zxq.co/ripple/rippleapi
git clone --recursive https://zxq.co/ripple/hanayo
git clone --recursive https://github.com/osuripple/old-frontend
git clone --recursive https://github.com/osufx/national-gallery
git clone --recursive https://github.com/osufx/secret
git clone --recursive https://github.com/osufx/ripple-python-common
```
API спиздить здесь - osu.ppy.sh/p/api

В конфигурации Nginx не забыть сертификаты

На сайт:
```
git clone https://github.com/Neilpang/acme.sh.git
cd acme.sh
./acme.sh --issue --standalone -d osu.aestival.space -d c.aestival.space -d a.aestival.space -d oldripple.aestival.space
```

```
$ php path/to/ripple/ci-system/migrate.php
```

```
php /path/to/ripple/osu.ppy.sh/cron.php
```

Не забываем про конфигурации серверов:
- osu.ppy.sh - основной сервер
- a.ppy.sh - сервер с аватарками
- c.ppy.sh - Bancho
- c1.ppy.sh - запасной Bancho

Конфигурация
```
sudo nano /etc/vsftpd.conf (allow write for local users)
sudo mysql_secure_installation
mysql -u root -p
GRANT ALL PRIVILEGES ON *.* TO 'username' IDENTIFIED BY 'password';
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf (comment "bind-address = 127.0.0.1")
```

Для админ панели:
```
composer install
nano inc/config.php
git clone --recursive https://github.com/osufx/secret
```

пипрки > скор
```
set "score-overwrite": "pp" at common/config.json
```

Питоновские загрузки
```
$ pip install flask 
$ pip install tornado
$ pip install pymysql
$ pip install psutil
$ pip install bcrypt
```

Под запуск:
```
$ python3 /path/to/ripple/c.ppy.sh/pep.py
```

Новый пользователь
```
adduser %name%
usermod -aG sudo %name%
```

Вкл/Выкл/Рестарт
```
sudo systemctl enable vsftpd
sudo systemctl enable nginx
sudo systemctl enable php7.0-fpm
sudo systemctl enable redis-server
sudo systemctl enable mysql
sudo systemctl start vsftpd
sudo systemctl start nginx
sudo systemctl start php7.0-fpm
sudo systemctl start redis-server
sudo systemctl start mysql
sudo systemctl restart vsftpd
sudo systemctl restart nginx
sudo systemctl restart php7.0-fpm
sudo systemctl restart redis-server
sudo systemctl restart mysql
```


# 30.06.19
Запилил сервер на собственном компе. 
Тестирую фронт-энд сайта, пока к нему невозможно подключится.

Также наладил скачивание карт, они будут грузиться на прямую с серверов ppy, osu!direct нот воркинг.

----------

Конфиг nginx под перепись:
```
# Web frontend/PHP backend
server {
    listen 80;
    listen 443 ssl;    # required for stable/beta/cutting edge support
    server_name ripple.moe osu.ppy.sh www.osu.ppy.sh;
    root /path/to/ripple/osu.ppy.sh;

    # required for stable/beta/cutting edge support
    ssl on;
    ssl_certificate /path/to/ppy.sh.cert.pem;
    ssl_certificate_key /path/to/ppy.sh.key.pem;

    ...

    # PHP FastCGI
    location ~ \.php$ {
        ...
    }

    # nginx rewrite
    location / {
        rewrite ^/(u|d)/[0-9]+$ /rewrite.php;
    }
}

# Avatar server
server {
    listen 80;
    listen 443 ssl;    # required for stable/beta/cutting edge support
    server_name a.ppy.sh a.ripple.moe;

    # required for stable/beta/cutting edge support
    ssl on;
    ssl_certificate /path/to/ppy.sh.cert.pem;
    ssl_certificate_key /path/to/ppy.sh.key.pem;

    root /path/to/ripple/a.ppy.sh;
    client_max_body_size 5M;
    client_body_buffer_size 256K;
    try_files $uri @shout;

    location @shout {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto https;
        proxy_set_header Host $http_host;
        proxy_redirect off;
        proxy_pass http://127.0.0.1:5000;    # default port is 5000
    }
}

# Bancho server
server {
    listen 80;
    listen 443 ssl;    # required for stable/beta/cutting edge support
    server_name c.ppy.sh c1.ppy.sh c.ripple.moe;

    # required for stable/beta/cutting edge support
    ssl on;
    ssl_certificate /path/to/ppy.sh.cert.pem;
    ssl_certificate_key /path/to/ppy.sh.key.pem;

    root /path/to/ripple/c.ppy.sh;
    client_max_body_size 5M;
    client_body_buffer_size 256K;
    try_files $uri @shout;

    location @shout {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto https;
        proxy_set_header Host $http_host;
        proxy_redirect off;
        proxy_pass http://127.0.0.1:5001;    # default port is 5001
    }
}
```
