---
layout: post
title: Getting Started
categories:
    - from-scratch
---

Getting started with Orchestra Platform 3 is as much identical to getting started with Orchestra Platform 2. Therefore you need to have some minimum knowledge on [Laravel 5](http://laravel.com), [Composer](https://getcomposer.org) and [Packagist](https://packagist.org).

I would highly recommend watching [Laravel 4 From Scratch](https://laracasts.com/series/laravel-5-from-scratch) video series from [Laracasts](https://laracasts.com) in order to get familiar with Laravel 5 to understand some of the approach we use in Orchestra Platform 3.

<!--more-->

## Video

<iframe width="720" height="540" src="//www.youtube.com/embed/CMA_7k375a4?rel=0&amp;vq=hd720" frameborder="0" allowfullscreen></iframe>

## Instruction

```bash
$ composer create-project orchestra/platform patio dev-master --prefer-dist --dev
```

> Once Orchestra Platform v3.0 is released, you can swap `dev-master` with `3.0.x`.

This composer command would create a new project for you on `patio` folder using the latest development build, `--prefer-dist` is another option that you can use to indicate that you want to download a distributed version instead of cloning the repository, otherwise use `--prefer-source`.

```bash
$ cd patio
$ php artisan serve
Laravel development server started on http://localhost:8000
```


