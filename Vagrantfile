# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  config.vm.box = "debian/bullseye64"

    #Creación servidor DHCP 
    config.vm.define "dhcp-server" do |server|
    server.vm.hostname = "dhcp-server"
    server.vm.network "private_network", ip: "192.168.56.10"
    server.vm.network "private_network", 
                      ip: "192.168.57.10", 
                      virtualbox__intnet: "intNet1",
                      auto_config: true
    server.vm.network "forwarded_port", guest: 22, host: 2222, id: "ssh"
    #server.vm.provision "ansible" do |ansible|
    # ansible.playbook = "playbooks/dhcp-server.yaml"
    # ansible.inventory_path = "inventory.yaml"
    #end
  end

  #Creacion servidor DNS
  config.vm.define "dns-server" do |dns|
    dns.vm.hostname = "dns-server"
    dns.vm.network "private_network", ip: "192.168.56.20"
    dns.vm.network "private_network",
               ip: "192.168.57.20",
               virtualbox__intnet: "intNet1",
               auto_config: true
    dns.vm.network "forwarded_port", guest: 22, host: 2201, id: "ssh"
    #dns.vm.provision "ansible" do |ansible|
    # ansible.playbook = "playbooks/dns-server.yaml"
    # ansible.inventory_path = "inventory.yaml"
    #end
  end


  #Cliente 1
  config.vm.define "c1" do |c1|
    c1.vm.hostname = "c1"
    c1.vm.network "private_network",
                  virtualbox__intnet: "intNet1",
                  type: "dhcp"
    c1.vm.network "forwarded_port", guest: 22, host: 2200, id: "ssh"
    c1.vm.provision "ansible" do |ansible|
     ansible.playbook = "playbooks/client.yaml"
     ansible.inventory_path = "inventory.yaml"
    end
  end
end
