#### Original repository
    https://github.com/lightweightdjango/examples

U:demo
P:demo1234

#### First time pre-steps
```text
(*) -> run first time only
/etc/hosts -> 127.0.0.1	db
bash_profile -> alias d-c="docker-compose" alias d-r="docker-compose run web"
```

#### Setup Environment (*)
```bash
mkvirtualenv --python=/Library/Frameworks/Python.framework/Versions/3.6/bin/python3 lightweight_django_01
sudo easy_install pip
pip install --upgrade pip
pip install -r docker/requirements.txt
```

```text
set project interpreter to lightweight_django_01
```


#### Roll out
```bash
workon lightweight_django_01
docker rm `docker ps -a|grep lightweight_django_chapter8_db|cut -d' ' -f1`
rm -fr ./database
mkdir database
d-c up -d --build
docker exec -it `docker ps|grep lightweight_django_chapter8_db|cut -d' ' -f1` bash
createdb -E UTF-8 scrum -U postgres
exit
#update DATABASE in settings.py
d-c stop web
add to pycharm run profile -> runserver 0.0.0.0:8001
#python manage.py createsuperuserls
http://127.0.0.1:8000/
```

#### Clean up
```bash
d-c down
```
#### Diagnose
```bash
docker exec -it `docker ps|grep lightweight_django_chapter8_db|cut -d' ' -f1` bash
psql -U postgres
\l
\dt
\q

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'postgres',
        'USER': 'postgres',
        'HOST': 'db',
        'PORT': 5432,
    }
}
```