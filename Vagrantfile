# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don"t touch unless you know what you"re doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Configure The Box
  config.vm.box = "wfsneto/trusty64-php-apache"
  config.vm.hostname = "trusty64-php-apache"

  # Sync your folder on php folder
  config.vm.synced_folder "~/php", "/home/vagrant/php"

  # Configure A Private Network IP
  config.vm.network :private_network, ip: "192.168.10.10"

  # Configure A Few VirtualBox Settings
  config.vm.provider :virtualbox do |vb|
    vb.customize [ :modifyvm, :id, "--memory", 2048 ]
    vb.customize [ :modifyvm, :id, "--cpus", 1 ]
    vb.customize [ :modifyvm, :id, "--natdnsproxy1", "on" ]
    vb.customize [ :modifyvm, :id, "--natdnshostresolver1", "on" ]
  end

  # Configure Port Forwarding To The Box
  config.vm.network :forwarded_port, guest: 80, host: 8080
  config.vm.network :forwarded_port, guest: 8000, host: 8000
  config.vm.network :forwarded_port, guest: 3306, host: 33060
  config.vm.network :forwarded_port, guest: 5432, host: 54320

  # Configure The Public Key For SSH Access
  config.vm.provision :shell do |s|
    s.inline = "echo $1 | tee -a /home/vagrant/.ssh/authorized_keys"
    s.args = [File.read(File.expand_path("~/.ssh/id_rsa.pub"))]
  end

  # Copy The SSH Private Keys To The Box
  key = "~/.ssh/id_rsa"
  config.vm.provision :shell do |s|
    s.privileged = false
    s.inline = "echo \"$1\" > /home/vagrant/.ssh/$2 && chmod 600 /home/vagrant/.ssh/$2"
    s.args = [File.read(File.expand_path(key)), key.split('/').last]
  end
end