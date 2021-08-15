---
title: Flask
tags:
  - Flask
  - Flask-SQLalchemy
  - Flask-RESTful
  - Python
  - MySQL
abbrlink: f70621bd
date: 2020-12-24 17:40:35
---

# Flask

**Flask is a lightweight Web application framework written in Python.**

## Install

```python
pip install flask

# test
from flask import Flask
app = Flask(__name__)


@app.route('/')
def hello_world():
    return 'Hello World'


if __name__ == '__main__':
    app.run(debug = True)
```

## Structure

[**Flask is very flexible, it has no certain pattern of a project folder structure.**](https://lepture.com/en/2018/structure-of-a-flask-project)

- Functional Based Structure

  ```python
  project
   ├── __init__.py
   ├─models
   |   ├── base.py
   |   ├── users.py
   |   ├── posts.py
   |   └── __init__.py
   ├─routes
   |   ├── home.py
   |   ├── account.py
   |   └── __init__.py
   ├─templates
   |   ├── base.html
   |   └── post.html
   ├─services
   |   ├── google.py
   |   ├── mail.py
   |   └── __init__.py
  ```

- App Based Structure

  ```python
  project
   ├── db.py
   ├── main.py
   ├── __init__.py
   ├── source
   ├─auth
   |   ├── models.py
   |   ├── route.py
   |   ├── templates.py
   |   └── __init__.py
   ├─blog
   |   ├── models.py
   |   ├── route.py
   |   ├── templates.py
   |   └── __init__.py
   pip freeze > requirements.txt
   pip install -r requirements.txt
  ```

## Connect MySQL

[Flask-sqlalchemy Reference](http://www.pythondoc.com/flask-sqlalchemy/)

```python
# setting.py		Some configurations for connecting to the database
DIALECT = 'mysql'
DRIVER = 'pymysql'
USERNAME = 'root'
PASSWORD = '123456'
HOST = 'localhost'
PORT = '3306'
DATABASE = 'test'
SQLALCHEMY_DATABASE_URI = '{}+{}://{}:{}@{}:{}/{}?charset=utf8'.format(
  DIALECT, DRIVER, USERNAME, PASSWORD, HOST, PORT, DATABASE
)
SQLALCHEMY_TRACK_MODIFICATIONS = True

# run.py		Read the configurations
import seetings
from project import create_app
if __name__ == '__main__':
    app = create_app()
    app.config.from_object(seetings.DevelopmentConfig)
    app.run()

# db.py		Create an instance of SQLAlchemy
from flask_sqlalchemy import SQLAlchemy
db = SQLAlchemy()
def init_app(app):
    global db
    db = SQLAlchemy(app)

# project/__init__.py		Pass the Flask instance into the db instance
from flask import Flask
def routes(app):
    from . import test
    test.init_app(app)
def create_app():
    from . import db
    app = Flask(__name__)
    db.init_app(app)
    routes(app)
    return app

# models.py		Create the modelsViews
from project.db import db
class Car(db.Model):
    __tablename__ = 'car'
    carId = db.Column(db.INTEGER, primary_key=True)
    carName = db.Column(db.String(10))
    @property
    def serialize(self):
        return to_json(self, self.__class__)

#	routes.py		Register the Blueprint
from .views import *
def init_app(app):
    app.register_blueprint(test_bp, url_prefix='/test')


# models/__init__.py		Register the Blueprint
from flask import Blueprint
test_bp = Blueprint('test', __name__)
def init_app(app):
    from . import routes
    routes.init_app(app)
    print(app.url_map)

# views.py		Deploy the data interface
from . import test_bp
from .models import Car
@test_bp.route('/data', methods=['GET', ])
def get_data():
    res = Car.query.get(3)
    print(res.serialize)
    data = {
        'data': res.serialize,
        'status': 'success'
    }
    return data
```

### Issue

- ImportError: No module named flask.ext.sqlalchemy

  ```python
  pip install flask-sqlalchemy

  from flask_sqlalchemy import SQLAlchemy
  ```

- The query results convert to JSON

  [use the Flask-RESTful]()

## Requirement

```python
 pip freeze > requirements.txt
 pip install -r requirements.txt
```

## References

[Flask Reference](https://www.w3cschool.cn/flask/)

[Case project](https://github.com/smaIIstars/STA)
