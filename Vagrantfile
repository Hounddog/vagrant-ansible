Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu-precise12042-x64-vbox43"
  config.vm.box_url = "https://puphpet.s3.amazonaws.com/ubuntu-precise12042-x64-vbox43.box"
  config.vm.synced_folder "./www", "/var/www", id: "vagrant-root", :nfs => false
  config.vm.usable_port_range = (2200..2250)
  config.ssh.keep_alive = true
  config.vagrant.host = :detect

  #Local webserver environment
  config.vm.define :web1, primary:true do |web1|
    web1.vm.network "private_network", ip: "192.168.56.105"  
    web1.vm.provider :virtualbox do |virtualbox|
      virtualbox.customize ["modifyvm", :id, "--name", "webserver1"]
      virtualbox.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      virtualbox.customize ["modifyvm", :id, "--memory", "512"]
      virtualbox.customize ["setextradata", :id, "--VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
    end
    web1.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/playbook.yml"
      ansible.inventory_path = "./provisioning/inventory/webservers"
    end
  end

  #Local database server environment
  config.vm.define :db1, primary:true do |db1_config|
    db1_config.vm.network "private_network", ip: "192.168.56.110"
    db1_config.vm.synced_folder ".", "/vagrant", disabled: true
    db1_config.vm.provider :virtualbox do |virtualbox|
      virtualbox.customize ["modifyvm", :id, "--name", "dbserver1"]
      virtualbox.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      virtualbox.customize ["modifyvm", :id, "--memory", "512"]
    end
    db1_config.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/playbook.yml"
      ansible.inventory_path = "./provisioning/inventory/dbservers"
    end
  end
end