---
title: Flask-SQLalchemy
tags:
  - Flask
  - Flask-SQLalchemy
  - MySQL
  - Python
abbrlink: 18e27ceb
date: 2020-12-24 17:44:23
---

# Flask-SQLalchemy

**Flask-SQLAlchemy is an extension for Flask that adds support for SQLAlchemy to your application.**

## a quick example

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:////tmp/test.db'
db = SQLAlchemy(app)


class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True)
    email = db.Column(db.String(120), unique=True)

    def __init__(self, username, email):
        self.username = username
        self.email = email

    def __repr__(self):
        return '<User %r>' % self.username

@app.route('/data', methods=['GET', ])
def get_data():
    # ...
    return data
```

## [Select, Insert, Delete](https://flask-sqlalchemy.palletsprojects.com/en/2.x/queries/#select-insert-delete)

### Querying Records

- [Select](https://blog.csdn.net/xiao_bao_an/article/details/84667705)

  ```python
  # Select the first
  User.query.first()
  User.query.get(1)   # According to primary key

  # Select all
  User.query.all()

  # Get the number of data quantity
  User.query.count()

  # Get the data with id 4
  User.query.filter_by(id=4).all()
  User.query.filter(User.id == 4).all()

  # startwith/endwith
  User.query.filter(User.name.startswith('xxx')).all()
  User.query.filter(User.name.endswith('xxx')).all()

  # contain
  User.query.filter(User.name.contains("n")).all()
  User.query.filter(User.name.like("%n%g")).all()
  User.query.filter(User.id.in_([1, 3, 5, 7, 9])).all()

  # Intersection condition
  from sqlalchemy import and_
  User.query.filter(and_(User.name == 'small', User.age = 18)).all()

  # Union condition
  from sqlalchemy import or_
  User.query.filter(or_(User.name == 'small', User.age = 18)).all()

  # Not contain
  from sqlalchemy import not_
  User.query.filter(not_(User.name == 'small')).all()

  # order (Default ascending)
  User.query.order_by(User.age, User.id.desc()).limit(5).all()

  # Paging query
  """
  pn.items: The amount of data retrieved
  pn.page : The current page
  pn.pages: The amount of pages
  """
  pn = User.query.paginate(page, items)


  ```

## References

- [Official Website Reference](https://flask-sqlalchemy.palletsprojects.com/en/2.x/quickstart/)
