# Rails Dev Box

## Overview

**Rails Dev Box** is a ready to use vagrant machine. It ships with the following components:

**OS**: Ubuntu 12.04 LTS

**Git**

**Ruby**: 2.0.0-p247

**rbenv**

**Postgres**: 9.1

**MongoDB**

**Java**

**Imagemagick**

**Rails**: 4.0


### How to use

#### Clone this repo

    git clone git@github.com:agieche/rails-dev-box.git

#### Install dependencies

Install vagrant 1.3.5 (http://www.vagrantup.com/)
Install virtualbox 4.3 (https://www.virtualbox.org/)

#### Setup base box Box

    vagrant up # starts the vm and install all packages
    vagrant package # this creates a package.box file
    vagrant halt

Switch to your rails project folder:

    vagrant init my-project-box path/to/package.box

Add this to your Vagrantfile inside of the rails project folder:

    config.vm.network :forwarded_port, guest: 3000, host: 3000
    config.vm.network :private_network, ip: "10.10.10.10"
    config.vm.synced_folder ".", "/vagrant", :nfs => true

Save Vagrantfile and enter these commands inside of your rails project folder:

    vagrant up
    vagrant ssh

Now, you're on your vagrant machine. Finally do:

    cd /vagrant/
    bundle install
    bundle exec rake db:setup
    bundle exec rails server

Now, you're ready to go. Point your browser to http://localhost:3000 and get busy!



