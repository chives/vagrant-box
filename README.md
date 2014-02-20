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

### Prepare variables file

Copy ``vagrant/provisioning/variables.yml.dist`` file content to ``vagrant/provisioning/variables.yml``
Now you can change values of configuration variables to make your project more personal.

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
