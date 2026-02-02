# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.synced_folder ".", "/vagrant", disabled: true

  (0..2).each do |i|
    config.vm.define "controller-#{i}" do |master|
      master.vm.box = "bento/debian-12.9"
      master.vm.hostname = "controller-#{i}"
      master.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "2048"]
        vb.customize ["modifyvm", :id, "--cpus", "2"]
        vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      master.vm.network "public_network", :adapter=>2, bridge: 'Qualcomm QCA61x4A 802.11ac Wireless Adapter', ip: "192.168.50.20#{i}"
      end
    end
  end

  (0..2).each do |i|
    config.vm.define "worker-#{i}" do |node|
      node.vm.box = "bento/debian-12.9"
      node.vm.hostname = "worker-#{i}"
      node.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "2048"]
        vb.customize ["modifyvm", :id, "--cpus", "2"]
        vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      node.vm.network "public_network", :adapter=>2, bridge: 'Qualcomm QCA61x4A 802.11ac Wireless Adapter', ip: "192.168.50.21#{i}"
      end
    end
  end

  config.vm.provision "shell", inline: <<-SHELL
    export DEBIAN_FRONTEND=noninteractive 
    sudo apt-get update
    sudo apt-get install -y vim
    echo "set nocompatible" > /home/vagrant/.vimrc
    echo "set paste" >> /home/vagrant/.vimrc
  SHELL

  # config.vm.provision "ansible" do |ansible|
  #   ansible.playbook = "ansible/playbook.yml"
  #   ansible.groups = {
  #     "controllers" => ["controller-[0:2]"],
  #     "workers" => ["worker-[0:2]"]
  #   }
  # end
end
