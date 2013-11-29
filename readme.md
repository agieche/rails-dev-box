# Rails Dev Box

## Overview

**Rails Dev Box** is a preconfigured vagrant box, that you can use as base box for your different rails development vm needs.

The dependencies are managed via chef-solo and berkshelf.

The configuration of this base box ships with the following components:

**OS**:

- Ubuntu 12.04 LTS 64-bit

**Packages**:

- Git
- Ruby (2.0.0-p247 via rbenv)
- Node.js (for asset-pipeline)
- Java
- Imagemagick
- PhantomJS

**DB**:

- Postgres 9.1
- MongoDB

**Gems**:

- Bundler
- Rails

Feel free to add or remove chef cookbooks in the vagrantfile and berksfile to fit your apps requirements.

### How to use

#### Clone this repo

    git clone git@github.com:agieche/rails-dev-box.git

#### Install dependencies

Install vagrant 1.3.5 (http://www.vagrantup.com/)

Install virtualbox 4.3 (https://www.virtualbox.org/)

#### Install vagrant plugins

    vagrant plugin install --plugin-source https://rubygems.org --plugin-prerelease vagrant-vbguest
    vagrant plugin install vagrant-berkshelf
    vagrant plugin install vagrant-omnibus

#### Setup base box

    vagrant up # starts the vm and install all packages
    vagrant package # this creates a package.box file
    vagrant halt

#### Setup project specific vm

Switch to your rails project folder:

    vagrant init my-project-box path/to/package.box

Add this to your Vagrantfile inside of the rails project folder:

    config.vm.network :forwarded_port, guest: 3000, host: 3000
    config.vm.network :private_network, ip: "10.10.10.10"
    config.vm.synced_folder ".", "/vagrant", :nfs => true

#### Start project specific vm

Save Vagrantfile and enter these commands inside of your rails project folder:

    vagrant up
    vagrant ssh

#### Setup rails project inside of the vm

Now, you're on your vagrant machine. Finally do:

    cd /vagrant/
    bundle install
    bundle exec rake db:setup
    bundle exec rails server

Now, you're ready to go. Point your browser to http://localhost:3000 and get busy!

