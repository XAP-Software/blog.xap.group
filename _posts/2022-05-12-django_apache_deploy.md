---
layout: post
title: How to deploy a Django project using Apache?
categories: [Python, Django, Apache]
author_github: nevermore17
author: Verbin Kirill
---

You need to do some configuration of Apache and Django before deploying a django project to Apache.

# Install mod-wsgi library

The first step is to install `libapache2-mod-wsgi-py3` library to let Apache and Django communicate. `libapache2-mod-wsgi-py3` is an adapter for running python applications on the Apache web server.

```terminl

$ sudo apt-get install libapache2-mod-wsgi-py3

```

# Django configuration


## Install virtual env

Virtual env allows our project to work without modules conflict of other python projects. 

To install virtualenv type next command.

```
 $ sudo pip3 install virtualenv
```

Now we can create venv for our project and activate.

```
$ virtualenv env && ./env/bin/activate
```

After activate venv you need to install all modules for your project and create your project.

## Configuration settings.py and static 

Django requires collect static files.

```terminl
$ python3 manage.py collectstatic 
```

You need to necessary to register in the `settings.py` file where the folders with static files and media files is located.

```python
# settings.py


 ...

STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'static')

MEDIA_URL = "/media/"
MEDIA_ROOT = os.path.join(BASE_DIR,"media")

 ...
```

## Apache configuration

Create a `djangoproject.conf` file in the `/etc/apache2/sites-available/` directory and set the following settings:
 - standart host settings

```xml
        ServerAdmin localhost@webmaster
        ServerName server-name
        DocumentRoot /path/to/django/project
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
```
 - media and static folder settings

```xml
        Alias /static /path/to/django/project/static
        <Directory /path/to/django/project/static>
            Require all granted
        </Directory>

        Alias /media /path/to/django/project/media
        <Directory /path/to/django/project/media>
            Require all granted
        </Directory>
```
- wsgi file settings
```xml
        <Directory /path/to/django/project/my-project>
            <Files wsgi.py>
                Require all granted
            </Files>
        </Directory>
```
- venv settings
```apache
        WSGIDaemonProcess django_project python-path=/path/to/django/project python-home=/path/to/env/folder/env
        WSGIProcessGroup django_project
        WSGIScriptAlias / /path/to/django/project/my-project/wsgi.py
```

In the end you will get the following djangoproject.conf file:
```apache
<VirtualHost *:80>
        ServerAdmin localhost@webmaster
        ServerName server-name
        DocumentRoot /path/to/django/project
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        Alias /static /path/to/django/project/static
        <Directory /path/to/django/project/static>
            Require all granted
        </Directory>

        Alias /media /path/to/django/project/media
        <Directory /path/to/django/project/media>
            Require all granted
        </Directory>

        <Directory /path/to/django/project/my-project>
            <Files wsgi.py>
                    Require all granted
            </Files>
        </Directory>

        WSGIDaemonProcess django_project python-path=/path/to/django/project python-home=/path/to/env/folder/env
        WSGIProcessGroup django_project
        WSGIScriptAlias / /path/to/django/project/my-project/wsgi.py
</VirtualHost>
```
Activate this config and restart apache2 service.
```
$ sudo a2ensite testdjango.conf
$ sudo service apache2 restart
```




