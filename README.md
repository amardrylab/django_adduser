# Adding users in the django system from csv file

## 1) Create a django project
    django-admin startproject myproject .
    django-admin startapp myapp
    open myproject/settings.py and insert the "myapp" in the installed app section

## 2) Create a model in myapp/models.py

    from django.db import models

    # Create your models here.

    class AddUser(models.Model):
        email=models.EmailField()
        phone=models.CharField(max_length=12)
        name=models.CharField(max_length=30)

## 3) Run the following commands to create the database

    ./manage.py makemigrations
    ./manage.py migrate

## 4) Read your users.csv file in the database

    sqlite3 db.sqlite3
    .mode csv myapp_adduser
    .import users.csv myapp_adduser
    .save db.temp
    .quit
    rm db.sqlite3
    mv db.temp db.sqlite3

## 5) Transfer your data from myapp_adduser to auth_user table

    ./manage.py shell
    >>>from myapp.models import AddUser
    >>>users=AddUser.objects.all()
    >>>from django.contrib.auth.models import User
    >>>for user in users:
    ...User.objects.create_user(user.name, user.email, "GoponPass")
    ...
    >>>quit()

## 6) Run the following command and check http:127.0.0.1:8000/admin
    ./manage.py runserver

## Format of users.csv should be like the following

    0,firstname@gmail.com,999999999,firstname
    1,secondname@gmail.com,999999999,secondname
    2,thirdname@gmail.com,999999999,thirdname
    ...
