# osu! godmod private server
osu! private server dev repository.
Location - RU

Репозиторий состоит из библиотек и форков разного рода API, микро-инструкций и списка дел.
Дневник разработки также здесь.

# 29.06.19 - Старт

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

