Assignment 3

TASK 1-> To Dockerize a Django signup page with database

### Creating a Django sign-up application:
```shell
$ mkdir django_signup
$ cd django_signup

```
[Create a sample Django-signup page](https://github.com/ASHISH-KUMAR-PANDEY/Django_Adhoc)

 Dockerfile
```text
FROM python:3.6
ENV PYTHONUNBUFFERED 1
RUN mkdir /my_app_dir
WORKDIR /my_app_dir
ADD requirements.txt /my_app_dir/
RUN pip install -r requirements.txt
ADD . /my_app_dir/
```
docker-compose.yml
```yaml
version: '3'

services:
  db:
    image: mysql:5.7
    ports:
      - '3306:3306'
    environment:
       MYSQL_DATABASE: 'my-app-db'
       MYSQL_USER: 'root'
       MYSQL_PASSWORD: 'password'
       MYSQL_ROOT_PASSWORD: 'password'
  web:
    build: .
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/my_app_dir
    ports:
      - "8000:8000"
    depends_on:
      - db
```
 requirements.txt
```
appdirs==1.4.0
Django==1.10.5
packaging==16.8
pyparsing==2.1.10
six==1.10.0
mysqlclient==1.3.12
django-mysql==2.2.0
```
 /settings.py
```python
...
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'my-app-db',
        'USER': 'root',
        'PASSWORD': 'password',
        'HOST': 'db',
        'PORT': 3306,
    }
}
...
```
 Fire it up and Migrate the database:
```shell
$ docker-compose build
$ docker-compose up -d
$ docker-compose run web python manage.py migrate
```
----------------------
TASK 2->To Build A Docker Container That Executes Parent DE As The Container DE

Dockerfile
```text
FROM ubuntu
MAINTAINER ashishkumarpandey2000@gmail.com
RUN apt-get update && apt-get install apt-transport-https ca-certificates  curl  gnupg-agent  software-properties-common -y
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
RUN add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
RUN apt-get update && apt-get install docker-ce docker-ce-cli -y
```

docker-compose.yml
```yaml
version: '3.8'
services:
 tasktwo:
  build: .
  tty: true
  image: tasktwo
  container_name: tasktwoc1
  volumes:
   - /var/run/docker.sock:/var/run/docker.sock
  network_mode: host
```
Fire it up and Migrate the database:
```shell
$ docker-compose up --build -d
$ docker-compose exec tasktwo bash
$ docker images
$ docker ps -a
```
----------------------------------

TASK 3-> Automatically Update The Container Whenever The Image Update Occurs

```shell
$ sudo su
$ mkdir taskthree && cd taskthree
$ git clone https://github.com/microsoft/project-html-website.git
$ mv project-html-website webapp
$ atom Dockerfile
```
Dockerfile
```text
FROM httpd
MAINTAINER ashishpandey99@gmail.com
COPY ./web/ /usr/local/apache2/htdocs/
```
Shell
```shell
$ docker build -t ashishpandey/taskthree .
$ docker login
$ docker push ashishpandey/taskthree
$ docker run -itd --name autocont --network host ashishpandey/taskthree
```
```shell
$ docker run -d --name watchtower -v /var/run/docker.sock:/var/run/docker.sock v2tec/watchtower autocont
$ cd webapp
$ sed -i 's/Success/Failed/g' index.html
$ cd ..
$ docker build -t ashishpandey/taskthree .
$ docker push ashishpandey/taskthree
$ docker logs watchtower
```
----------------------------------------------
TASK 4->To Run Root Commands By A Non-Root User In A Docker Container

 Dockerfile
```text
FROM ubuntu
MAINTAINER ashishkumarpandey2000@gmail.com
RUN apt-get install sudo
RUN adduser --disabled-password --gecos '' ashish
RUN adduser ashish sudo
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
USER ashish
RUN sudo apt-get update 
```
Shell
```shell
$ docker build -t taskfour .
$ docker run -it --name taskfourc1 --network host taskfour
$ whoami
$ sudo apt-get install git
```
