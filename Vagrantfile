# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  os = "centos/7"
  net_ip = "192.168.50"

  config.vm.define :master, primary: true do |master_config|
    master_config.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
        vb.name = "master"
    end
    
    master_config.vm.box = "#{os}"
    master_config.vm.host_name = 'saltmaster.local'
    master_config.ssh.forward_agent = true
    master_config.vm.network "private_network", ip: "#{net_ip}.10"
    master_config.vm.synced_folder "saltstack/salt/", "/srv/salt"
    master_config.vm.synced_folder "saltstack/pillar/", "/srv/pillar"
    master_config.vm.synced_folder "saltstack/keys/", "/etc/salt/keys"

    master_config.vm.provision :salt do |salt|
      salt.master_config = "saltstack/etc/master"
      salt.master_key = "saltstack/keys/master.pem"
      salt.master_pub = "saltstack/keys/master.pub"
      salt.seed_master = {
                          "loadBalancer" => "saltstack/keys/loadBalancer.pub",
                          "webServer1" => "saltstack/keys/webServer1.pub",
                          "webServer2" => "saltstack/keys/webServer2.pub",
                          "dataBase" => "saltstack/keys/dataBase.pub",
                         }

      salt.install_type = "stable"
      salt.install_master = true
      salt.no_minion = true
      salt.verbose = true
      salt.colorize = true
      salt.bootstrap_options = "-P -c /tmp"
    end
  end


  [
    ["loadBalancer",    "#{net_ip}.11",    "512",    os ],
    ["webServer1",    "#{net_ip}.12",    "512",    os ],
    ["webServer2",    "#{net_ip}.13",    "512",    os ],
    ["dataBase",    "#{net_ip}.14",    "512",    os ],
  ].each do |vmname,ip,mem,os|
    config.vm.define "#{vmname}" do |minion_config|
      minion_config.vm.provider "virtualbox" do |vb|
          vb.memory = "#{mem}"
          vb.cpus = 1
          vb.name = "#{vmname}"
      end

      minion_config.vm.box = "#{os}"
      minion_config.vm.hostname = "#{vmname}"
      minion_config.vm.network "private_network", ip: "#{ip}"
      minion_config.vm.synced_folder "saltstack/keys/", "/etc/salt/keys"

      minion_config.vm.provision :salt do |salt|
        salt.minion_config = "saltstack/etc/#{vmname}"
        salt.minion_key = "saltstack/keys/#{vmname}.pem"
        salt.minion_pub = "saltstack/keys/#{vmname}.pub"
        salt.install_type = "stable"
        salt.verbose = true
        salt.colorize = true
        salt.bootstrap_options = "-P -c /tmp"
      end
    end
  end
end
