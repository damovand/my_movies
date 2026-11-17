## Compose sample application
### Python/Flask with Nginx proxy and MySQL database
Project structure:

.
├── compose.yaml
├── flask
│   ├── Dockerfile
│   ├── requirements.txt
│   └── server.py
└── nginx
    └── nginx.conf

compose.yaml

services:
  backend:
    build:
      context: backend
      target: builder
    ...
  db:
    # We use a mariadb image which supports both amd64 & arm64 architecture
    image: mariadb:10-focal
    # If you really want to use MySQL, uncomment the following line
    #image: mysql:8
    ...
  proxy:
    build: proxy
    ...
The compose file defines an application with three services proxy, backend and db. When deploying the application, docker compose maps port 80 of the proxy service container to port 80 of the host as specified in the file. Make sure port 80 on the host is not already being in use.

ℹ️ INFO
For compatibility purpose between AMD64 and ARM64 architecture, we use a MariaDB as database instead of MySQL.
You still can use the MySQL image by uncommenting the following line in the Compose file
#image: mysql:8

Deploy with docker compose
$ docker compose up -d
Creating network "nginx-flask-mysql_default" with the default driver
Pulling db (mysql:8.0.19)...
5.7: Pulling from library/mysql
...
...
WARNING: Image for service proxy was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Creating nginx-flask-mysql_db_1 ... done
Creating nginx-flask-mysql_backend_1 ... done
Creating nginx-flask-mysql_proxy_1   ... done
Expected resultAlpine’s package manager (similar to apt in Debian/Ubuntu or yum in CentOS).