---
layout: post
title: "Web In a Flask"
date: 2017-09-10
tag: Python
blog: true
author: chidonna
projects: false
summary: "Implementing a web app in the Flask framework"
headerImage: false
draft: false
---
![Web In a Flask]({{ site.url }}/assets/images/flask.jpg)
<div class="breaker"></div>

In this blog post, I will be going through the basic steps to take while trying to create a web application in Python, using a Python framework called Flask.

Flask is a microframework (a minimalist framework that can be extended with extensions) for Python based on Werkzeug (a WSGI utility library for Python). Been a micro framework does not mean it can not be used for large projects, rather it means it can be used for small projects with relative easy :smirk:, and can be structured to suit the needs of application with no strict measures on how your application should be written.

I will go through the methods, and steps I took while creating an e-commerce store and the various extensions I used. I won't teach you how to create an e-commerce store, neither will I teach you how to create a web application, rather I would show you just enough to get started with flask.

## Web Interface (Design):
[Bootstrap](http://getbootstrap.com/)!. Yes that's all, bootstrap, bootstrap and bootstrap. 

According to wikipedia, Bootstrap is a free and open-source front-end web framework for designing websites and web applications. It contains HTML- and CSS-based design templates for typography, forms, buttons, navigation and other interface components, as well as optional JavaScript extensions.

I have a barely functional knowledge of HTML, a "Hello World" knowledge of Javascript and no knowledge whatsoever of CSS, yet I was able to hack together a pretty decent Web page using Bootstrap. While I am not sure if Bootstrap would be so efficient in creating Single Page Applications, it would do a very good job for sites more concerned with content. Since teaching you how to use Bootstrap is not the purpose of this blog post, all you need to know to get started with Bootstrap can be found [here](http://www.tutorialspoint.com/bootstrap/).
> N.B Using templates can also form a very good base to start from. 

## Application structure:
Some people start working on an application in a single file, and then modularize as the application grows. I think this can lead to avoidable refactoring and less errors during development.

If you are not looking to create a large application, writing all your code in a single module would do, but even a small application can also get pretty messed up if everything is written in a single file. Therefore, to avoid unnecessary refactoring and fewer bashing of your head on the table, I encourage you to modularize your application. This application in this little tutorial would be written in modules.

In flask this is how it most likely would be structured:

```shell
├── app/
│   ├── auth/
│   ├── __init__.py
│   ├── main/
│   ├── models.py
│   ├── static/
│   └── templates/
├── config.py
├── db.sqlite
├── manage.py
├── README.md

```

Having your application structured this way makes it easy to work on different parts of the project, without having to change plenty things.

### Configuration
The **config.py** would contain the configuration variables your application requires to function. In this example it contains the location of the database to be used in the application (a SQlite3 database)

`project/config.py`

``` python
class Config(object):
    SQLALCHEMY_DATABASE_URI = 'sqlite:///<path to the database>' 

    @staticmethod
    def init_app(app):
        pass
        
class DevelopmentConfig(Config):
    DEBUG = True

class TestingConfig(Config):
    TESTING = True

# Configurations 
config = {
        'development': DevelopmentConfig
        'testing': TestingConfig
        
        'default': DevelopmentConfig
    }
```
In this `config.py` file, we have set the location to the database, a `DEBUG` boolean variable to `True`, for the `DevelopmentConfig` class. The "staticmethod" decorator used on the `init_app()`, is to create a similar method most flask extensions ship with.


### Application
To build a web application using the Flask framework, the first thing you need is a Flask to place all your eggs in :slight_smile:.

You would have to create an application factory. For the sake of this tutorial, this factory function would be called `create_app()`. This factory function would be responsible for creating multiple instance of the application, since the instances were created but not not initialized, `create_app()` would do this for the application.

The application factory would be located in the **app** directory in the **\_\_init__.py** file. We would make use of relative imports, which is indicating the location of the target module relative to the source. Using dot notation, where the first dot indicates the current directory and each subsequent dot represents the next parent directory.

> N.B Adding an \_\_init__.py to a folder makes it a package.

`project/app/__init__.py`

```python
from flask import Flask
from config import config   # from the config.py file created earlier

# any extensions to be used would be imported and an empty initialization would be made

# EXAMPLE

from flask_login import LoginManager

# empty initializations
login_manager = LoginManager()


def create_app(config_name):
    app = Flask(__name__)
    app.config.from_object(config[config_name])
    
    # the initializations are completed when the create_app() function is called
    
    config[config_name].init_app(app)
    
    login_manager.init_app(app)  # the login_manager is created at runtime
    
    return app
    
```

### Blueprints
Setting up the application this way would require the use of a special kind of operation in Flask called "Blueprints". Blueprints allow for great flexibility in your application, because these blueprints act as applications within the main applications. Focusing on the "main" directory that has already been created this would be a Blueprint, called "main". To complete this Blueprint, you would create an "\_\_init__.py" file in the "main" directory.

It should look like this:

`project/main/__init__.py`

```python
from flask import Blueprint

main = Blueprint('main', __name__)  # Blueprint instance

# using relative imports
from . import views                 # to avoid circular dependencies this is written below
```
The views function would be discussed later.

This main Blueprint that has been created would be registered in the `create_app()`  in the application factory (app/\_\_init__.y).

`project/app/__init__.py`

```python
# ...

def create_app(config_name):
    # ...
    
    # Blueprint registration
    from .main import main as main_blueprint   # relative imports
    app.register_blueprint(main_blueprint)
    
    return app
    
```

Blueprint is a good way to organize projects with several distinct components.

### Manager
The **manage.py** should contain the flask instance responsible for running the application. The **Flask-Script** extension is used to set up the manage.py. Flask-Script extensions provides support for writing external scripts in Flask, this includes a development server, a customised Python shell, scripts to set up your database and other command-line tasks that belong outside the web application itself.

`project/manage.py`

```python 
#!/usr/bin/env python3

from flask_script import Manager
from app import create_app

app = create_app('default')  # using the application factory and the configuration created in the config.py

manager = Manager(app)

# To add a command
@manager.command
def say_hello():
    print('Hello')
    
if __name__ = '__main__':
    manager.run()
```

With this setup to run the application, you would run:

`python manage.py runserver` 

Note the shebang at the top. If that is included and you converted to the file to an executable, you could simply run it as:

`./manage.py runserver` 

### Templates
This directory would contain the [jinja2](http://jinja.pocoo.org/) templates (web pages) for your application. From the index page, to the sign up page, to the login page, to the products page and every other page in the web application. Every web page to be rendered throughout the runtime of this application is to be kept in the templates folder, since this is where Flask searches for web pages to be rendered.

### Static Files
This directory contains the CSS, Javascript files and every other file that is to be viewed in the application. The images (like favicon) and other files not generated dynamically are stored in this folder. If you downloaded the Bootstrap files, they would be stored in this folder.

## Database
:cold_sweat: This is the arguably the most important aspect of a web application and definitely not the easiest to implement. You create models (tables), in those models you create columns and rows (columns are fixed, while rows vary depending on the number of entries in the model).

Databases can be implemented using a number of ORMs (Object-Relational Mappers), such as SQLAlchemy and peewee and others (maybe). For the sake of this tutorial SQLAlchemy will be used (no particular reason). Using an ORM removes plenty of the trouble of having to write SQL commands and queries directly. It provides a virtual object database that can be used within the programming language, thus you not been required to execute SQL commands, except on occasions where you choose to, or need to (Yh!, sometimes you might need to).

To the next one; Flask-SQLAlchemy, this adds support for SQLAlchemy to your application.
To add this to the application, in our **app/\_\_init__.py**

`project/app/__init__.py`

```python
# ... 
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()

def create_app(config_name):
    # ...
    db.init_app(app)     # initialization is completed at runtime
    
    return app
```

To create the models (tables), a **models.py** file would be created in the **app** folder.

This module would contain the models we want to create. You could create a one-to-many relationship, many-to-one relationship and a many-to-many relationship, depending on the purpose of your web application and the kind of data you want to store.

A one-to-many relationship means a table can have many users from another table while that table would have only one user from it, while a many-to-many relationship means a table can have many users from another table while that table can also have many users from it. 
To make this clearer let me use an example.

Take a Product and Category case. A product can have many categories while a category can have many products. To create a many-to-many relationship using the Product and Category scenario.

`project/app/models.py`

```python
from . import db   # using relative imports, we import db from this package in the __init__.py module


# Association Table
association_table = db.Table('association', 
                db.Column('products_id', db.Integer, db.ForeignKey(products.id))
                db.Column('categories_id', db.Integer, db.ForeignKey(categories.id))
                )


class Product(db.Model):
    __tablename__ = 'products'
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(20), index=True)
    category = db.relationship('Category',
                    secondary=association_table,
                    backref=db.backref('products', lazy='dynamic')
                    )

class Category(db.Model):
    __tablename__ = 'categories'
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(20), index=True)

```

In the snippet above two models were created, a Product model and a Category model and connected in a many-to-many relationship.

Unlike a one-to-many relationship where a table would have a relationship to another table and that table would have a column with a foreign key linking it to a particular column in the table it wants to reference, usually the id row (i.e the primary_key). In a many-to-many relationship, there would be an "Association" table linking both tables involved in the relationship as shown in the code above. This association table would contain as columns the tables involved in the relationship as columns.

The `db.Column()` would take three arguments, a name for the column, the type of value (since it's the primary key we are associating) and a `db.ForeignKey()` that would take as an argument the name of the row in the table it is linking to.

The association table is indicated by the secondary argument to the `db.relationship()` in a table, for a bidirectional relationship both models would contain the `db.relationship()`.

From the snippet above, the Product model is the parent and the Category model is the child. The Product model has a column with a `db.relationship()` that takes as its arguments the name of the child table and secondary argument that specifies the association table then a backref argument that specifies what this table is to be called if referenced from the Category model.

While this is very far from what an e-commerce application would require, this gives you an idea of how this would be implemented in python when using the Flask framework and SQLAlchemy to create your database models.

## Rendering Web pages:
Flask handles this very well without any extensions, using the `render_template()`. This function takes as arguments the name of the template to be rendered and other keyword arguments (or `**kwargs`). The `render_template()` is not called explicitly but is called within a "View" function.

Putting it mildly, View functions are functions that run a web page instance. Using a decorator `app.route()`, a function is transformed to a View function.

View functions are normally housed within a views.py module, but can also been moved to a folder, if you want to create many view functions for different purposes.

To create an _index_ view function.

`project/app/main/views.py`

```python
from flask import render_template
from . import main                  # import the Blueprint

# a view function for the index page in the main Blueprint
@main.route('/')
def index():
    return render_template('index.html')
    
# A view function for registering a user
@main.route('/register', methods=['GET', 'POST'])
def register():
    return render_template('register.html')
```

So when the website is opened as _www.example.com/_ the view function for the index is called and the index.html is rendered. When the url is *www.example.com/register*, the view function for the register is called. Take note that the name that follows the Top Level Domain name (www.example.com) is the same as the argument passed to the `main.route()` decorator.

Writing a login view is far more complicated than that. I will show you how to create a proper login system using a flask extension called
"Flask-Login".

## Login System
Creating a login system is pretty straight forward using another flask extension called **Flask-Login** to handle the logins and **WTForms** and **Flask-WTF** to handle the forms to be presented in the application. I would show you how to create a basic form to register and login a user to a web application.

#### Access Granted
You must first import the Flask-Login extension in to the application and initialize it, you would have to create an instance of the **LoginManager** from the Flask-Login extension:

`project/app/__init__.py`

```python
# ...
from flask_login import LoginManager   # import the LoginManager

# ...

login_manager = LoginManager()    # create an instance of the LoginManager

def create_app(config_name):
    # ...
    
    login_manager.init_app(app)    # initialize at runtime
    
    # ...
    
    return app
```

#### Into the DB
Creating a database model to store the data gotten from the registration is essential, because during login, this data is queried before a user can be logged in.

`project/app/models.py`

```python
from datetime import datetime

from flask_login import UserMixin
from werkzeug.security import check_password_hash, generate_password_hash
from . import db, login_manager

# UserMixin is also inherited so the model can have the login property
class User(db.Model, UserMixin):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(20), index=True, unique=True)
    password_hash = db.Column(db.String(128))
    prog_lang = db.Column(db.String(20), index=True)
    specialization = db.Column(db.String(20))
    created = db.Column(db.Datetime, default=datetime.utcnow())

    @property
    def password(self):
        raise AttributeError('Go away, I will not show you this password')
        
    @password.setter
    def password(self, password):
        self.password_hash = generate_password_hash(password)
        
    def verify_password(self, password):
        return check_password_hash(self.password_hash, password)
    

# Flask-Login requires a callback function that loads the user given the id
@login_manager.user_loader
def load_user(user_id):
    return User.query.get(int(user_id))

```
The "property" decorator used on the password function, indicates that the password is a property (an attribute) and should not be treated as a method of the class. 

The `AttributeError()` raised is to prevent a password string from being viewed within the application. The "password.setter" decorator makes use of the "generate_password_hash" gotten from the "Werkzeug" library to create a hash. So instead of the password string been stored in the database, a hash is stored.

The `verify_password` function takes as an argument the password string, uses the "check_password_hash" (also gotten from Werkzeug) to check if the password is the same as the hash stored in the database, if it is the same, it returns `True`.

The `login_manager.user_loader` decorator around the `load_user()`, acts a callback function that loads the identifier of the user when a login wants to be made.

#### WTF
Now, you create the forms using "WTForms" and "Flask-WTF":

`project/app/main/forms.py`

```python
from flask_wtf import FlaskForm
from wtforms import StringField, SubmitField, TextAreaField, PasswordField
from wtforms.validators import Required, EqualTo, Length


# A form for registering new users
class RegistrationForm(FlaskForm):
    
   name = StringField('Full name', validators=[Required()]) 
   password = PasswordField('Password', validators=[Required(), Length(min=6) EqualTo(fieldname='password2', message='passwords must be the same')])
   password2 = PasswordField('Confirm Password', validators=[Required()])
   prog_lang = StringField('Programming Language', validators=[Required()])
   specialization=StringField('Where do you specialize', validators=[Required()])
   submit = SubmitField('Submit')
   
# A form to login users
class LoginForm(FlaskForm):

    name = StringField('Full name', validators=[Required()])
    password=PasswordField('Enter Password', validators=[Required()])
    remember_me = BooleanField('Keep me logged in')
    submit = SubmitField('Login')
```

Using a parent class **FlaskForm** from the "Flask-WTF" extension, forms can be created by directly inheriting from this "FlaskForm".

The "wtforms" package comes with validators, and some sort of pre-written fields, that are used to determine the kind of data to be received by the form. The validators are checkers that determine if the data entered matches the required specification before the form is sent, if it does not fit the requirements, the form is rejected and error messages are raised. Validators can be used to check the length of the data, matched against a regular expression, and even compared with another field.

#### Views from the Flask
Creating the view functions to handle this registration of new users, and the login of users.

`project/app/main/views.py`

```python
from flask import render_template, redirect, url_for
from .. import db
from .forms import RegistrationForm
from ..models import User

@main.route('/register', methods=['GET', 'POST'])
def register():
    form = RegistrationForm
    if form.validate_on_submit():
        user = User(
                name=form.name.data, prog_lang=form.prog_lang.data, 
                password=form.password.data, specialization=form.specialization.data)
        db.session.add(user)
        db.session.commit()
        redirect (url_for('main.index'))
    return render_template('index.html', form=form)
    

@main.route('/login', methods=['GET', 'POST'])
def login():
    form = LoginForm
    if form.validate_on_submit():
        user = User.query.filter_by(email=form.name.data).first()
        if user is not None and user.verify_password(form.password.data):
            login_user(user, form.remember_me.data)
            return redirect(url_for('main.index'))
    return render_template('login.html', form=form)

```

#### Jinja
Finally, we create the templates:

Creating the templates is not really what you would be expecting if you coming from a HTML background. In Flask, templates are created using the jinja2 templating system.

For the login view function:

`project/app/templates/login.html`

For the register view function:

`project/app/templates/register.html`

I can't show you any snippet of the jinja2 templating system and how it works, because it keeps messing with the liquid templating used on this blog. Check out the documentation [here](http://jinja.pocoo.org/docs/latest/).


## Conclusion
This tutorial does not go through every detail involved in writing a web application, but it does goes through enough to get you started, so don't go about looking for the most polished tutorial in the world, GET STARTED !!!

> The best way to learn is by doing
>> -- <cite> John Sonmez </cite>

Using a framework such as "Django" might seem like a straight forward go to when deciding which framework to go with, but Flask has its own advantages.

Thank you for taking the time to read this, if you have anything, you would like to say me concerning flask, python or anything software, feel free to [contact me](mailto:pogbonna34@gmail). You can also send me a tweet. If you feeling particularly charitable, you can follow me on twitter or github just to see what am up to :wink:.

