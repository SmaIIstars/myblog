---
title: Flask-RESTful
tags:
  - Flask
  - Flask-RESTful
  - Python
  - MySQL
abbrlink: 85306f1d
date: 2020-12-24 17:42:53
---

# Flask-RESTful

**Flask-RESTful is an extension for Flask that adds support for quickly building REST APIs.**

## Install

```python
pip install flask-restful
```

## Basic Usage

[Case project](https://github.com/smaIIstars/STA)

```python
test
 ├── models.py
 ├── routes.py
 ├── views.py
 └── __init__.py

# models.py		Create the SQLalchemy modelViews
class Car(db.Model):
    __tablename__ = 'car'
    carId = db.Column(db.INTEGER, primary_key=True)
    carName = db.Column(db.String(10))

# views.py		Create the RESTful modelViews
from flask_restful import Resource, fields, marshal_with
from .models import Car
class CarView(Resource):
    resource_fields = {
        'name': fields.String(attribute='carName'),
        'id': fields.Integer(attribute='carId'),
    }
    @marshal_with(resource_fields)
    def get(self):
        data = Car.query.first()
        print(data.carId)
        return data
    def post(self):
        pass

 # routes.py		Register the Blueprints and add the resources
from .views import *
from flask_restful import Api
from . import test_bp
def add_resources(api):
    api.add_resource(CarView, '/data')
def register_blueprints(app):
    app.register_blueprint(test_bp, url_prefix='/test')

def init_app(app):
    api = Api(test_bp)
    register_blueprints(app)
    add_resources(api)

```

## Renaming Attributes

```python
resource_fields = {
  'name': fields.String(attribute='DatabaseName'),
  'id': fields.Integer(attribute='DatabaseId'),
}
```

## Default Values

```python
fields = {
    'name': fields.String(default='SmallStars'),
    'address': fields.String,
}
```

## Fields

- String (_default=None_, _attribute=None_)

- FormattedString (_src_str_)

  ```python
  # If the key is not found in the object, returns the default value.
  fields.FormattedString("Hello {name}")
  ```

- Url (_endpoint=None_, _absolute=False_, _scheme=None_, _\*\*kwarg_)

  - **endpoint** (_str_) – Endpoint name. If endpoint is `None`, `request.endpoint` is used instead
  - **absolute** (_bool_) – If `True`, ensures that the generated urls will have the hostname included
  - **scheme** (_str_) – URL scheme specifier (e.g. `http`, `https`)

- DateTime (_dt_format='rfc822'_, _\*\*kwargs_)

  - **dt_format** (_str_) – `'rfc822'` or `'iso8601'`

- Float (_default=None_, _attribute=None_)

- Integer (_default=0_, _\*\*kwargs_)

- Nested (_nested_, _allow_null=False_, _\*\*kwargs_)

- List (_cls_or_instance_, _\*\*kwargs_)

- Boolean (_default=None_, _attribute=None_)

- Fixed (_decimals=5_, _\*\*kwargs_)

## marshal_with and marshal

- marshal_with Return the processed data directly

  ```python
  # @marshal_with(car_fields)
  class CarView(Resource):
      car_fields = {
          'name': fields.String(attribute='carName'),
          'id': fields.String(attribute='carId')
      }

      @marshal_with(car_fields)
      def get(self):
          data = Car.query.all()
          return data
  ```

- marshal Can return the custom data structure

  ```python
  class CarView(Resource):
      car_fields = {
          'name': fields.String(attribute='carName'),
          'id': fields.String(attribute='carId')
      }

      # @marshal_with(car_fields)
      def get(self):
          data = Car.query.all()
          data = marshal(data, CarView.car_fields)
          return {
              'data': data,
              'status': 'successful'
          }
  ```

## References

- [Official Website Reference](https://flask-restful.readthedocs.io/en/latest/index.html)
- [ORM: object relational mapping](https://baike.baidu.com/item/%E5%AF%B9%E8%B1%A1%E5%85%B3%E7%B3%BB%E6%98%A0%E5%B0%84/311152?fromtitle=ORM&fromid=3583252&fr=aladdin)

- [Understanding of endpoint in Flask](https://blog.csdn.net/hello_albee/article/details/51638358)
- [Combine the Flask-RESTful and blueprint](https://blog.csdn.net/qq_42517220/article/details/88870177)
- [Format of Flask-RESTful](https://blog.csdn.net/qq_41134008/article/details/105666432)
- [flask-restful-extend](https://github.com/anjianshi/flask-restful-extend)
- [Summary of Flask-RESTful](https://www.cnblogs.com/donghaoblogs/p/10389696.html)
