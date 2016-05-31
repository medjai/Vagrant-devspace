# Laravel Vagrant

Laravel Vagrant is a simple __Ubuntu Trusty64__ vagrant configuration for LAMP stack developers and includes modern development tools. Vagrant hostmanager is a plugin 
utilized in this project to  automatically create host entries across OSX, Linux, 
and Windows (**Please see attached notes below for windows, extra configuration is required for promptless use**)

## Overview
This vagrant use [ubuntu/trusty64](https://atlas.hashicorp.com/ubuntu/boxes/trusty64) from [Atlas Vagrant Box](https://atlas.hashicorp.com/boxes/search?utm_source=vagrantcloud.com&vagrantcloud=1).
  On your 'vagrant up' command, this vagrantfile will automatically download the box. Project contains a `bootstrap.sh` file which provisions the vagrant box to 
  automate the installation and configuration of packages. It will also automatically create a new laravel project in the HOST folder `www` which 
  is linked to GUEST folder `/var/www/html`.


This vagrant box is configured to use `2048MB` of RAM and `2` CPU core. This can be changed from `config.yml`.

### Installation Pre-requesites
1. [Vagrant](http://vagrantup.com) 
2. [VirtualBox](https://www.virtualbox.org)

### Configuration Pre-requesites
1. Install [Vagrant Host Manager] (https://github.com/devopsgroup-io/vagrant-hostmanager). This is used to auto configure your host file when you spin up a VM.
2. `$ vagrant plugin install vagrant-hostmanager`
 * Windows Users: To avoid the UAC prompt, open ```%WINDIR%\System32\drivers\etc\``` in Explorer, right-click the hosts file, go to Properties > Security > Edit and give your user Modify permission.
 * Due to limitations caused by UAC, cancelling out of the UAC prompt will not cause any
visible errors, HOWEVER the ```hosts``` file will *NOT* be updated!

### Included packages

- Ubuntu Trusty64 (64-Bit)
- Apache 2
- PHP _v5.5.9_ with mysql, mcrypt, memcached, memcache, sqlite, xmlrpc, geoip gd, xdebug, php5-fpm, php5-common
- MySQL _v5.5.49_
- Git _v1.9.1_
- Node _v0.10.25_ (NodeJS Legacy as well)
- Composer _v1.1.1 2016-05-17_
- NPM _v1.3.10_
- Bower _v1.7.9_
- Gulp _v3.9.1_
- cURL
- python-software-properties
- build-essential
- libev-dev

 
# Usage
Clone the git repository: 

    $ git clone http://git.wisenetdev.com:medjai/vagrant.git

Navigate to the repository folder

    $ cd vagrant
Copy [config.yml.example](config.yml.example) to `config.yml` and edit parameters as you wish


    $ cp config.yml.example config.yml
    $ nano config.yml

Now you will be ready to launch the VM

    $ vagrant up

First time you `vagrant up` it will provision the VM. At the end of provisioning you will find a laravel instance already installed in the `www` folder



# Default Credentials

## Host Address:
- Host: 192.168.33.11 (Change this in `config.yml`)
 
## SSH
- Username: vagrant
- Password: vagrant
- Port: 22

## MySQL Credentials
- Username: root
- Password: root
- Host: localhost
- Port: 3306

`NOTE:` You can externally connect to MySQL from your *HOST* as it is listening on 0.0.0.0`

### Allow MySQL root user connection from any host 
`WARNING:`This is never to be done a production server. This is soley to easily execute `php artisan migrate` and other commands from your *HOST* during development.


    $ vagrant ssh
    $ mysql -uroot -proot

This will drop you into mysql prompt logged in as root

    $mysql> use mysql;
    $mysql> update user set `host`='%' where `user`='root' and `host`='::1';


You should see ` Query OK, 1 row affected (0.00 sec) Rows matched: 1  Changed: 1  Warnings: 0`

### Create Laravel database

    $mysql> CREATE DATABASE laravel CHARACTER SET utf8 COLLATE utf8_general_ci;

`Query OK, 1 row affected (0.00 sec)`

### Edit .env file for laravel
You can now edit your `.env` file to connect to MySQL

    $ nano /var/www/html/.env
    
```
DB_CONNECTION=mysql
DB_HOST=(change this to the host in your _config.yml_ file)
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=root

```