# Laravel Vagrant

Laravel Vagrant is a simple __Ubuntu Trusty64__ vagrant configuration for LAMP stack developers which also includes many related modern development tools.

# [Vagrant Host Manager] (https://github.com/devopsgroup-io/vagrant-hostmanager)
Vagrant hostmanager is a plugin used to automatically create host entries across OSX, Linux, and Windows (please see attached notes below for windows)

## Vagrant Host Manager Installation
------------
Install the plugin following the typical Vagrant 1.1 procedure:

    $ vagrant plugin install vagrant-hostmanager

To update the host's `/etc/hosts` file, set the `hostmanager.manage_host` attribute to `true`.

Example configuration:

```ruby
Vagrant.configure("2") do |config|
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.manage_guest = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = false
  config.vm.define 'example-box' do |node|
    node.vm.hostname = 'example-box-hostname'
    node.vm.network :private_network, ip: '192.168.42.42'
    node.hostmanager.aliases = %w(example-box.localdomain example-box-alias)
  end
end
```
Windows support
---------------

Hostmanager will detect Windows guests and hosts and use the appropriate
path for the ```hosts``` file: ```%WINDIR%\System32\drivers\etc\hosts```

By default on a Windows host, the ```hosts``` file is not writable without
elevated privileges. If hostmanager detects that it cannot overwrite the file,
it will attempt to do so with elevated privileges, causing the
[UAC](http://en.wikipedia.org/wiki/User_Account_Control) prompt to appear.

To avoid the UAC prompt, open ```%WINDIR%\System32\drivers\etc\``` in
Explorer, right-click the hosts file, go to Properties > Security > Edit
and give your user Modify permission.

### UAC limitations

Due to limitations caused by UAC, cancelling out of the UAC prompt will not cause any
visible errors, however the ```hosts``` file will not be updated.

# Configuration Pre-requesites
Configuration for Vagrant box has been simplified by using a `config.yml`. Just simply copy `config.yml.example` and edit the required fields.

## Example config.yml
```yaml 
vm:
  host_name: laravel.lan
  ip_address: 192.168.33.11
  name: "Laravel Test Project"
  memory: 2048
  cpu_cores: 2
  http_port: 8080 #if port conflicts with another VM vagrant file has been configured to re-configure to another port
  mysql_port: 3333  #if port conflicts with another VM vagrant file has been configured to re-configure to another port
```

# Overview
This vagrant use [ubuntu/trusty64](https://atlas.hashicorp.com/ubuntu/boxes/trusty64) from [Atlas Vagrant Box](https://atlas.hashicorp.com/boxes/search?utm_source=vagrantcloud.com&vagrantcloud=1).
  On your 'vagrant up' command, this vagrantfile will automatically download the box. Vagrant folder here contain a `bootstrap.sh` file which provision the vagrant box.
  
  You need to place your projects in `projects` directory. This directory is synced with `/var/www/html` directory in the virtual machine. 
  This project folder also contain a `config` folder which is used during the provisioning. 

This vagrant box is configured to use '1024mb' of RAM and 1 Cpu. You can change ths configuration from Vagrantfile.
 
## Included packages

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

### Included Dependencies
The following dependencies are installed using apt-get as they are required to install and build other modules:

- cURL
- python-software-properties
- build-essential
- libev-dev

 
# Installation

### Install via Git
To use Laravel Vagrant, clone this github repo 

    $ git clone http://git.wisenetdev.com:medjai/vagrant.git

to your mac/pc/linux.  When clone is complete, go to the `vagrant` folder and now you are ready to use your VM.

# Usage
Start the VM

    $ cd /vagrant
    $ vagrant up

First time you 'vagrant up' it will provision the VM. At the end of provisioning you will find a laravel instance already installed in the _*www*_ folder

### Requirements
You must have [Vagrant](http://vagrantup.com) and [VirtualBox](https://www.virtualbox.org) installed in your pc.


# Default Credentials
These are credentials setup by default.

## Host Address:
- Hose: 192.168.33.11 (Change in Vagrantfile if you like)
 
## SSH
- Username: vagrant
- Password: vagrant
- Port: 22

## MySQL Credentials
- Username: root
- Password: root
- Host: localhost
- Port: 3306
` Note: you can externally connect to MySQL from your host as it is listening on 0.0.0.0 just be sure to edit the DB to allow root user to enter from anywhere not just localhost or 127.0.0.1`
