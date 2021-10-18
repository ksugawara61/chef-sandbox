# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANT_API_VERSION = '2'.freeze

node_ip = '172.16.1.170'
node_hostname = 'node001'
node_vm_memory = 2048
node_vm_cpus = 2
node_port_array = [80, 443, 3000, 3001, 3002, 3003, 9200]

Vagrant.configure(VAGRANT_API_VERSION) do |config|
  config.vm.box = "ubuntu/bionic64"

  config.vm.hostname = node_hostname
  node_port_array.each do |port|
    config.vm.network 'forwarded_port', guest: port, host: port
  end
  config.vm.network 'private_network', ip: node_ip
  config.vm.provider :virtualbox do |vb|
    vb.memory = node_vm_memory
    vb.cpus = node_vm_cpus
  end
  config.vm.synced_folder(
    './',
    '/var/chef',
    owner: 'root',
    group: 'root'
  )

  config.vm.provision 'shell', privileged: false, inline: <<-SHELL
    sudo apt-get update
    wget https://packages.chef.io/files/stable/chef-workstation/21.9.613/ubuntu/20.04/chef-workstation_21.9.613-1_amd64.deb -q
    sudo dpkg -i chef-workstation_21.9.613-1_amd64.deb
    rm chef-workstation_21.9.613-1_amd64.deb
    chef -v
  SHELL
end
