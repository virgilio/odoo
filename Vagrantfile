# -*- mode: ruby -*-
# vi: set ft=ruby :

USER = "vagrant"

$runserver = <<SCRIPT
    # must run with vagrant user
    # cd /vagrant
    cd /home/vagrant/odoo

    tmux -2 new-session -d -s vagrant -n 'odoo'
    tmux send-keys "./odoo-bin --config=odoo-config" C-m

SCRIPT

Vagrant.configure('2') do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.provision :shell, path: "scripts/bootstrap-ubuntu.sh", privileged: false, keep_color: true
  # config.vm.provision :shell, path: "scripts/production-ubuntu.sh", privileged: false, keep_color: true
  config.vm.provision "shell",
           inline: $runserver,
           privileged: false,
           run: "always"
  config.vm.network "forwarded_port", guest: 8069, host: 8069
  config.vm.provider "virtualbox" do |v|
    	v.memory = 1024
     	v.cpus = 2
  end
  config.ssh.username = USER
  config.vm.synced_folder "./", "/home/" + USER  + "/odoo/", create: true
  config.vm.synced_folder "~/dev/custom", "/home/" + USER  + "/odoo-modules/", create: true
end
