# -*- mode: ruby -*-
# vi: set ft=ruby :
# export VAGRANT_EXPERIMENTAL="disks"

OS = "bento/centos-8.4"

Vagrant.configure("2") do |config|

   config.vm.define "pxeserver" do |server|
     server.vm.box = OS
     server.vm.disk :disk, size: "15GB", name: "extra_storage1"
   
     server.vm.host_name = 'pxeserver'
     server.vm.network :private_network, 
                        ip: "10.0.0.20", 
                        virtualbox__intnet: 'pxenet'
   
     server.vm.network "forwarded_port", guest: 80, host: 8081
   
     server.vm.provider "virtualbox" do |vb|
       vb.memory = "1024"
       vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
     end
   
   #   # ENABLE to setup PXE
   #   server.vm.provision "shell",
   #     name: "Setup PXE server",
   #     path: "scripts/setup_pxe.sh"
       server.vm.provision "ansible" do |ansible|
         ansible.playbook = "ansible/playbook.yml"
       end
     end
   
   # config used from this
   # https://github.com/eoli3n/vagrant-pxe/blob/master/client/Vagrantfile
     config.vm.define "pxeclient" do |pxeclient|
       pxeclient.vm.box = OS
       pxeclient.vm.host_name = 'pxeclient'
       pxeclient.vm.network :private_network, 
                              ip: "10.0.0.21",
                              virtualbox__intnet: 'pxenet'

       pxeclient.vm.provider :virtualbox do |vb|
         vb.memory = "4096"
         vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
         vb.customize [
             'modifyvm', :id,
             '--nic1', 'intnet',
             '--intnet1', 'pxenet',
             '--nic2', 'nat',
             '--boot1', 'net',
             '--boot2', 'none',
             '--boot3', 'none',
             '--boot4', 'none'
           ]
       vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
       end
         # ENABLE to fix memory issues
      #  end
     end
   
   end
   