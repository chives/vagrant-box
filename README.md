# Installation

First of all clone this repo into ``vagrant`` folder in your project's root directory. Then follow instructions below

## Vagrantfile

Copy file ``vagrant/Vagrantfile.dist`` to ``vagrant/Vagrantfile`` and change "PROJECT-NAME" in it to something uniquely
identifying your project.

## Default puppet manifest

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

## Install dependincies

Then you can install dependencies from composer.json file if you don't have them already.
This can be done in 2 different ways, from host machine and from vm machine.

### Install vendors from host machine (recommended) 

This should be done by developers that needs vendors for IDE code suggestions. Its also much faster than
installing deps from VM.

```
& composer.phar install
```

### Install vendors from virtual machine (not recommended) 

```
$ cd vagrant
$ vagrant ssh
$ cd /var/www/symfony2_app
$ COMPOSER_VENDOR_DIR=/var/tmp/vendor composer install --dev --prefer-dist --no-scripts
$ ln -s /var/tmp/vendor vendor
```

## Install application's schema, assets, etc

```
$ vagrant ssh
$ cd /var/www/symfony2_app
$ php app/console doctrine:schema:create --env="test"
$ php app/console ca:cl --env="test"
$ php app/console assets:install web --symlink
```

## /etc/hosts

Now you should add project domain (``$host_name`` from default.pp file) to /etc/hosts file at your host machine. 

```
10.0.0.200      'symfony-project.dev'
```

From now you should be able to access site at host machine under http://symfony-project.dev/ address.
