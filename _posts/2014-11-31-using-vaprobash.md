---
layout: post
title: Using Vaprobash
categories: from-scratch
previous_page: from-scratch/installation

---

Before we go any further, I going to introduce to you how to include [Vaprobash](https://github.com/fideloper/Vaprobash) to the course development stack. Other than Vaprobash, you could also look into [Homestead](http://laravel.com/docs/4.2/homestead) to manage your virtual machine (VM).

<!--more-->

## Video

<iframe width="720" height="540" src="//www.youtube.com/embed/UFiC4Os_9ys?rel=0&amp;vq=hd720" frameborder="0" allowfullscreen></iframe>

## Instruction

To get started, go to your project directory and run the following command:

```bash
$ wget -O Vagrantfile http://bit.ly/vaprobash
```

If you don't have `wget` installed, you could also opt for `curl`:

```bash
$ curl -L http://bit.ly/vaprobash > Vagrantfile
```

Once the download is completed, you should open `Vagrantfile` using your favourite IDE and start customising the option.

### Setup VM

> This setup should be unique for every project.

#### 1. Hostname

Change the value of `hostname` to be relevant to your project. E.g:

```ruby
hostname = "patio.app"
```

#### 2. Server IP

Change the value of `server_ip` which is not currently used. E.g:

```ruby
server_ip = "192.168.50.10"
```

#### 3. Public Directory

Change the value of `public_folder` to Orchestra Platform 3 public folder, by default this would be under `public` directory:

```ruby
public_folder = '/vagrant/public'
```

#### 4. Change VM Name

(Optional) but you should be able to set a customise name for your VM:

```ruby
# If using VirtualBox
config.vm.provider :virtualbox do |vb|

   vb.name = "Patio"
```

and avoid Ubuntu from losing internet connection:

```ruby
# Prevent VMs running on Ubuntu to lose internet connection
   vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
   vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
```

### Setup Base Packages

Now, it's time to customise the set of packages to be provisioned on the VM. For this course we will be using:

* Nginx
* PHP 5.6
* MariaDB
* Redis
* Beanstalkd
* Supervisord
* Node.js
* Composer
* Mailcatcher

> You could also create additional provision bash script and include that to `Vagrantfile`.

### Provision

Once your `Vagrantfile` has been configured, all you need to do is run:

```bash
$ vagrant up
```

### Setup Database on the VM

Once provision is completed (it going to take awhile). SSH to the VM and create a new database:

```bash
$ vagrant ssh
```

Once logged in:

```bash
vagrant@patio:~$ mysql -uroot -proot

MariaDB [(none)]> CREATE database `patio`DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> quit
```

Alternatively as mention before, you can create a provision script to automate this process, E.g:

```bash
#!/usr/bin/env bash

/usr/bin/mysql -uroot -proot -e "CREATE DATABASE IF NOT EXISTS patio DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;"

cd /vagrant/

composer install --prefer-dist --dev
```

### Running from VM

Once all above has been completed, we can start browsing from VM, to do so just use the server ip followed by `.xip.io` to access the service, in this case it would be <http://192.168.50.10.xip.io>. You can also modify your local machine hosts to create an alias.

```bash
$ sudo vim /etc/hosts
```

And add the following:

```
192.168.50.10 patio.app
```

Now you should be able to access the application via <http://patio.app>.

### Setup Environment

Once your environment is ready, let start configuring environment variable for the app. First of all let change the default `production` environment to `local`. To do this you need to create a new `.env` file on the root directory of the project.

```ruby
APP_ENV=local
```

You can also convert the database configuration (from previous tutorial) to use environment variables:

```ruby
APP_ENV=local
DB.HOST=127.0.0.1
DB.USERNAME=root
DB.PASSWORD=root
DB.DATABASE=patio
```

Now, let's change `config/database.php`:

```php
<?php

return [

    // ...

    'connections' => [

        // ...

        'mysql' => [
            'driver'    => 'mysql',
            'host'      => getenv('DB.HOST') ?: 'localhost',
            'database'  => getenv('DB.DATABASE') ?: 'patio',
            'username'  => getenv('DB.USERNAME') ?: 'root',
            'password'  => getenv('DB.PASSWORD') ?: 'root',
            'charset'   => 'utf8',
            'collation' => 'utf8_unicode_ci',
            'prefix'    => '',
        ],

        // ...

    ],

    // ...

];
```

> Don't forget to delete the default `config/local/database.php`, you might need to run `vagrant reload`, or restart PHP service on the VM (to flush the OpCache).
