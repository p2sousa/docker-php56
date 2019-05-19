# docker-php56
Environment for development of multiple projects using php in version 5.6 and mysql database

1 - clone repository
````
$ git clone https://github.com/p2sousa/docker-php56.git
````

2 - copy files examples and modify content `projects`
````
$ cp docker-compose.yml.example docker-compose.yml
$ cp .env.dist .env
$ cp docker/nginx/app.conf.example docker/nginx/app.conf
```