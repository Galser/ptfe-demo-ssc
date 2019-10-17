# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "alvaro/bionic64"
  config.vm.hostname = "ptfe"
  config.vm.network "private_network", ip: "192.168.56.22"
  config.vm.provision "file", source:"~/Downloads/tfe/andrii-hashicorp-emea.tar", destination: "$HOME/"
#  config.vm.provision "shell", privileged: false, path: "scripts/bootstrap.sh"

  config.vm.provider "virtualbox" do |v|
    v.name = "ptfe-demo" # for VirtualBoix identification
    v.memory = 4096
    v.cpus = 2    
  end  

#  config.ssh.insert_key = false
  
end