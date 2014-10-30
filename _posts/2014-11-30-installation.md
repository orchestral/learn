---
layout: post
title: Getting Started
categories:
    - from-scratch
---

Getting started with Orchestra Platform 3 is as much identical to getting started with Orchestra Platform 2. Therefore you need to have some minimum knowledge on [Laravel 5](http://laravel.com), [Composer](https://getcomposer.org) and [Packagist](https://packagist.org).

I would highly recommend watching [Laravel 5 From Scratch](https://laracasts.com/series/laravel-5-from-scratch) video series from [Laracasts](https://laracasts.com) in order to get familiar with Laravel 5 to understand some of the approach we use in Orchestra Platform 3.

<!--more-->

## Video

<iframe width="720" height="540" src="//www.youtube.com/embed/CMA_7k375a4?rel=0&amp;vq=hd720" frameborder="0" allowfullscreen></iframe>

## Instruction

```bash
$ composer create-project orchestra/platform patio dev-master --prefer-dist --dev
```

> Once Orchestra Platform v3.0 is released, you can swap `dev-master` with `3.0.x`.

This composer command would create a new project for you on `patio` folder using the latest development build, `--prefer-dist` is another option that you can use to indicate that you want to download a distributed version instead of cloning the repository, otherwise use `--prefer-source`.

### Setup database

Next, login to MySQL client. For this example I'm using the default MySQL command line tool and create a new `patio` database:

```bash
$ mysql -uroot -p

mysql> create database `patio` DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
Query OK, 1 row affected (0.00 sec)

mysql> quit
```

### Changing the configuration file

Open `config/database.php` from any of your favourite IDE and edit the following based on your database connection credential:

```php
<?php

return [

    // ...

    'connections' => [

        // ...

        'mysql' => [
            'driver'    => 'mysql',
            'host'      => 'localhost',
            'database'  => 'patio',
            'username'  => 'root',
            'password'  => 'root',
            'charset'   => 'utf8',
            'collation' => 'utf8_unicode_ci',
            'prefix'    => '',
        ],

        // ...

    ],

    // ...

];
```



### Using the built in PHP Server

This is an easy approach to start using Orchestra Platform 3 without using either Apache or Nginx.

```bash
$ cd patio
$ php artisan serve
Laravel development server started on http://localhost:8000
```

> In the next tutorial, we will look into using Vaprobash.

### Installation

Now browse to <http://localhost:8000>, you should see the default Laravel 5 welcome page. Now let install Orchestra Platform, to do that simply browse to <http://localhost:8000/admin> and complete the installation process.
