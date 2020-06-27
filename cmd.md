# Capstone project

## Dependencies

```sh
pip freeze > requirements.txt
```

## Environment Configuration

heroku web interface

## Gunicorn

a pure-Python HTTP server for WSGI applications

in Procfile

```Procfile
web: gunicorn app:app
```

## Database Manage & Migrations on Heroku

```sh
pip3 install flask_script
pip3 install flask_migrate
pip3 install psycopg2-binary
```

manage.py

```py
from flask_script import Manager
from flask_migrate import Migrate, MigrateCommand

from app import app
from models import db

migrate = Migrate(app, db)
manager = Manager(app)

manager.add_command('db', MigrateCommand)


if __name__ == '__main__':
    manager.run()
```

heroku run command for you

```sh
python3 manage.py db init
python3 manage.py db migrate
python3 manage.py db upgrade
```

```sh
heroku create name_of_your_app
```

```sh
git remote add heroku heroku_git_url.
```

```sh
heroku addons:create heroku-postgresql:hobby-dev --app name_of_your_application
```

```sh
heroku config --app name_of_your_application
```

```sh
git push heroku master
```

```sh
heroku run python manage.py db upgrade --app name_of_your_application
```
