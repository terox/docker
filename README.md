# Docker files
> An easy and ready Docker files ready to use in your projects

## How to use with ```docker-compose```

1. Add this repository as [git submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules):

    ```sh
    git submodule add https://www.github.com/terox/docker docker
    ```

2. Create a ```docker-compose.yml``` in your project root and add the services that you want use, for example
the PHP 7.4:

    ```yaml
    version: '2'

    services:

        nginx:
            container_name: 'myproject.nginx'

            build: 
                context: docker/service/nginx1.18.x

            args:
                php_container: 'myproject.php'
                preset: 'symfony4.conf'


            ports:
                - 8089:80

            links:
                - php

            volumes_from:
                - php    

        php:
            container_name: 'myproject.php'

            build: 
                context: docker/service/php7.4-fpm

            args:
                timezone: 'Europe/Madrid' # or you preferred timezone

            volumes:
                - .:/var/www/application 
    ```

3. Ready to play:

    ```sh
    docker-compose up
    ```
