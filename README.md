# Description
FSi Vagrant box is full stack vagrant configuration for any existing Symfony2 based project. Its ready to use after
very simple installation.  
It includes following software: 

* PHP 5.4.19 
* MySQL 5.5.32
* GIT 1.7.9.5
* Apache 2.2.22
* Vim
* MC (Midnight commander)
* Curl
* Java (installed only when ``$install_selenium`` var in manifests/default.pp have "true" value)
* Selenium (installed only when ``$install_selenium`` var in manifests/default.pp have "true" value)
* Firefox (installed only when ``$install_selenium`` var in manifests/default.pp have "true" value)

# Installation

Clone this repo into ``vagrant`` folder in your project's root directory and remove .git folder

```
$ git clone git@github.com:fsi-open/vagrant-box.git vagrant && rm -rf vagrant/.git
```

### Prepare Vagrantfile

Copy file ``vagrant/Vagrantfile.dist`` to ``vagrant/Vagrantfile`` and change "PROJECT-NAME" in it to something
uniquely that identify your project.

### Prepare puppet manifest

Copy ``vagrant/manifests/default.pp.dist`` file content to ``vagrant/manifests/default.pp``  
Now you can change values of configuration variables to make your project more personal.

* ``$host_name = "symfony-project.dev"`` - project domain (it will be used in /etc/hosts) 
* ``$db_name = "symfony"`` - production database, username and pasword (can't be longer than 12 characters) 
* ``$db_name_dev = "${db_name}-dev"`` - dev database, username and pasword (can't be longer than 12 characters)
* ``$db_name_tst = "${db_name}-tst"`` - test database, username and pasword (can't be longer than 12 characters)
* ``$install_selenium = false`` - change ``false`` to ``"true"`` if you want to install java and selenium

For the above configuration you should have following configuration in app/config/parameters.yml file 

```yml
parameters:
    database_driver: pdo_mysql
    database_host: 127.0.0.1
    database_port: null
    database_name: symfony
    database_user: symfony
    database_password: symfony
    mailer_transport: smtp
    mailer_host: 127.0.0.1
    mailer_user: null
    mailer_password: null
    locale: en
    secret: ThisTokenIsNotSoSecretChangeIt
```

### Install dependencies

Then you can install dependencies from composer.json file if you don't have them already.

#### Install vendors from host machine (recommended) 

This should be done by developers that needs vendors for IDE code suggestions. Its also much faster than
installing deps from VM.

```
& composer.phar install
```

### /etc/hosts

Now you should add project domain (``$host_name`` from default.pp file) to /etc/hosts file at your host machine. 

```
10.0.0.200      'symfony-project.dev'
```
From now you should be able to access site at host machine under http://symfony-project.dev/ address.
