# Installation

First of all clone this repo into ``vagrant`` folder in your project's root directory. Then follow instructions below

## Vagrantfile

Copy file ``vagrant/Vagrantfile.dist`` to ``vagrant/Vagrantfile`` and change "project-name" in it to something uniquely
identifying your project.

## Default puppet manifest

Copy file ``vagrant/manifests/default.pp.dist`` to ``vagrant/manifests/default.pp`` and change project's name in the
first line. This must be the same name as you used in ``Vagrantfile``. This name will be used in some puppet modules
configurations such as apache virtual host, directory of project's files and default databases names inside VM.

**Important! Project's name cannot be longer than 12 character.**

Please remember that this name must be used in all following instructions in place of ``project-name``.

## Install dependincies

Then you can install dependencies from composer.json file if you don't have them already.
This can be done in 2 different ways, from host machine and from vm machine.

### When you can install vendors at host machine

This should be done by developers that needs vendors for IDE code suggestions. Its also much faster than
installing deps from VM.

```
& composer.phar install --dev
```

### When you can't install vendors at host machine

```
$ cd vagrant
$ vagrant ssh
$ cd /var/www/project-name
$ COMPOSER_VENDOR_DIR=/var/tmp/vendor composer install --dev --prefer-dist --no-scripts
$ ln -s /var/tmp/vendor vendor
```

## Install application's schema, assets, etc

```
$ vagrant ssh
$ cd /var/www/project-name
$ php app/console doctrine:schema:create --env="test"
$ php app/console ca:cl --env="test"
$ php app/console assets:install web --symlink
```

## /etc/hosts

Now you should add to /etc/hosts at you host machine following line:

```
10.0.0.100      project-name.dev
```

From now you should be able to access site at host machine under http://project-name.dev/ address.
