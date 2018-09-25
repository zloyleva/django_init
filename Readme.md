# Python 3.6 and Django 2.1

1. Create an empty project directory.

2. Create a new file called Dockerfile in your project directory.

3. Add the following content to the Dockerfile.
```sh
FROM python:3
ENV PYTHONUNBUFFERED 1
RUN mkdir /code
WORKDIR /code
ADD requirements.txt /code/
RUN pip install -r requirements.txt
ADD . /code/
```
4. Create a requirements.txt in your project directory.
```sh
Django>=2.1
psycopg2
```
5. Create a file called docker-compose.yml
```code
version: '3'

services:
  db:
    image: postgres
  web:
    build: .
    command: python3 manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/code
    ports:
      - "8000:8000"
    depends_on:
      - db
```
6. Create the Django project by running(composeexample - project name)
```sh
sudo docker-compose run web django-admin.py startproject composeexample .
```
7. Change the ownership of the new files.
```sh
sudo chown -R $USER:$USER .
```
8. In your project directory, edit the composeexample/settings.py file.
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'postgres',
        'USER': 'postgres',
        'HOST': 'db',
        'PORT': 5432,
    }
}
```
9. Run:
```sh
sudo docker-compose up
```