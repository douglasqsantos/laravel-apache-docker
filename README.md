## Laravel Base with Docker

*Dockerfile* consists of basic apache document root config, *mod_rewrite* and *mod_header*, *composer*, *node*, *npm*, *yarn* and sync container's uid with host uid.

*docker-compose.yml* boots up *php-apache* (mount app files) and *mysql* (mount db files), using *networks* to interconnect.

## How to use
- Checkout the project 
- Configure the .env
- Execute the docker-compose
- Execute the composer

Configure the .env
```
cp .env.example .env
```

Executing the compose to build the image 
```
docker-compose build
```

Executing the compose to start the containers
```
docker-compose up -d
```

Executing the compose to check out the logs
```
docker-compose logs -f
```

Executing the composer install 
```
./composer install
```

Generate the key for Laravel
```
./php-artisan key:generate
```

Now we can access the Laravel: http://localhost:8080/

## Helpers 

From time to time, I want to be able to quickly run CLI commands (*composer*, *artisan*, etc.) without having to type **docker exec** everytime. So here are some bash scripts I made wrapping around **docker exec**:

## container

Running **./container** takes you inside the laravel-app container under user uid(1000) (same with host user)

```bash
./container 
devuser@2fe14786d508:/var/www/html$ exit
exit
```

## db 

Running **./db** will connect to your database container's daemon using mysql client.

```sql
./db
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.28 MySQL Community Server (GPL)

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> \q
Bye
```

## composer

Run any *composer* command, example:
```
./composer dump-autoload
Generating optimized autoload files> Illuminate\Foundation\ComposerScripts::postAutoloadDump
> @php artisan package:discover --ansi
Discovered Package: facade/ignition
Discovered Package: fideloper/proxy
Discovered Package: laravel/tinker
Discovered Package: laravel/ui
Discovered Package: nesbot/carbon
Discovered Package: nunomaduro/collision
Package manifest generated successfully.
Generated optimized autoload files containing 3885 classes
```

## php-artisan

Run *php artisan* commands, example:
```bash
./php-artisan make:controller NewBlogPostController --resource
php artisan make:controller NewBlogPostController --resource
Controller created successfully.
```

## phpunit

Run *./vendor/bin/phpunit* to execute tests, example:
```
./phpunit --group=failing
vendor/bin/phpunit --group=failing
PHPUnit 8.5.1 by Sebastian Bergmann and contributors.



Time: 35 ms, Memory: 6.00 MB

No tests executed!
```


## Update for Laravel 6: The New Way (php artisan make:auth)

Basically, **make:auth** command was used to create the authentication scaffolding. The concept has not been removed, but the way of implementation has been changed

Authentication support is now added with the help of a package now [More details](https://laravel.com/docs/6.x/authentication)

The command to implement Auth is as follows:

```
composer require laravel/ui
php artisan ui vue --auth
```

This command will install a layout view, registration and login views, as well as routes for all authentication end-points. A HomeController will also be generated to handle post-login requests to your application's dashboard.

**NOTE:** If your Login and Register page only shows plain HTML. And CSS is not loading properly then run this two command:
```
npm install
npm run dev
```

## References
- [dev.to](https://dev.to/veevidify/docker-compose-up-your-entire-laravel-apache-mysql-development-environment-45ea)
- [make:auth](https://stackoverflow.com/questions/57774231/artisan-command-makeauth-is-not-defined-in-laravel-6)
- [php:7.2-apache](https://github.com/docker-library/php/blob/873725e57ec2fc5f2642dc0023676597bcc4bea9/7.2/stretch/apache/Dockerfile)
- [node:10.18-stretch-slim](https://github.com/nodejs/docker-node/blob/3c10e908934690b6af4f8f83b7e5e1da49926b34/10/stretch-slim/Dockerfile)
