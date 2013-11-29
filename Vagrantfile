# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "rails-dev-box"

  # The url from where the 'config.vm.box' box will be fetched if it
  # doesn't already exist on the user's system.
  config.vm.box_url = "https://opscode-vm-bento.s3.amazonaws.com/vagrant/opscode_ubuntu-12.04_provisionerless.box"

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network :forwarded_port, guest: 80, host: 8080

  # rails specific network settings
  config.vm.network :forwarded_port, guest: 3000, host: 3000
  config.vm.network :private_network, ip: "10.10.10.10"
  config.vm.synced_folder ".", "/vagrant", :nfs => true

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "4096"]
  end

  config.vbguest.auto_update = true
  config.berkshelf.enabled = true
  config.omnibus.chef_version = :latest
  config.vm.provision :chef_solo do |chef|

    chef.log_level = :info

    chef.add_recipe "apt"
    chef.add_recipe "git"
    chef.add_recipe "build-essential"
    chef.add_recipe "nfs"
    chef.add_recipe "ruby_build"
    chef.add_recipe "rbenv::vagrant"
    chef.add_recipe "rbenv::user"
    chef.add_recipe "nodejs"
    chef.add_recipe "java"
    chef.add_recipe "postgresql::client"
    chef.add_recipe "postgresql::server"
    chef.add_recipe "postgresql::libpq"
    chef.add_recipe "mongodb::10gen_repo"
    chef.add_recipe "mongodb"
    chef.add_recipe "imagemagick"
    chef.add_recipe "phantomjs"

    chef.json = {
      'rbenv' => {
        'user_installs' => [
          {
            'user' => 'vagrant',
            'rubies' => ['2.0.0-p247'],
            'global' => '2.0.0-p247',
            'gems' => {
              '2.0.0-p247' => [
                { 'name' => 'bundler' },
                { 'name' => 'rails' },
              ]
            }
          }
        ]
      },
      "postgresql" => {
        enable_pgdg_apt: true,
        "version" => "9.1",
        config: {
          ssl: false,
        },
        pg_hba: [
          { type: "local", db: "all", user: "postgres",   addr: "",             method: "trust" },
          { type: "local", db: "all", user: "all",        addr: "",             method: "trust" },
          { type: "host",  db: "all", user: "all",        addr: "127.0.0.1/32", method: "trust" },
          { type: "host",  db: "all", user: "all",        addr: "::1/128",      method: "trust" },
          { type: "host",  db: "all", user: "postgres",   addr: "127.0.0.1/32", method: "trust" },
          { type: "host",  db: "all", user: "username",   addr: "127.0.0.1/32", method: "trust" }
        ]
      },
    }
  end
end
