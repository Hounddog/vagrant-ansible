Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu-precise12042-x64-vbox43"
  config.vm.box_url = "https://puphpet.s3.amazonaws.com/ubuntu-precise12042-x64-vbox43.box"
  config.vm.synced_folder "./", "/var/www", id: "vagrant-root", :nfs => false
  config.vm.usable_port_range = (2200..2250)
  config.ssh.keep_alive = true
  config.vagrant.host = :detect

  #Local development environment
  config.vm.define :dev, primary:true do |dev|
    dev.vm.network "private_network", ip: "192.168.56.102"  
    dev.vm.provider :virtualbox do |virtualbox|
      virtualbox.customize ["modifyvm", :id, "--name", "development"]
      virtualbox.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      virtualbox.customize ["modifyvm", :id, "--memory", "512"]
      virtualbox.customize ["setextradata", :id, "--VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
    end
    dev.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/playbook.yml"
      ansible.inventory_path = "./provisioning/inventory_development"
    end
  end

  #Local staging webserver environment
  config.vm.define :stage_web, primary:true do |stage_web_config|
    stage_web_config.vm.network "private_network", ip: "192.168.56.200"
    stage_web_config.vm.synced_folder ".", "/vagrant", disabled: true
    stage_web_config.vm.provider :virtualbox do |virtualbox|
      virtualbox.customize ["modifyvm", :id, "--name", "webserver1"]
      virtualbox.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      virtualbox.customize ["modifyvm", :id, "--memory", "512"]
    end
    stage_web_config.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/playbook.yml"
      ansible.inventory_path = "./provisioning/inventory_staging"
    end
  end

  #Local staging dbserver environment
  config.vm.define :stage_db, primary:true do |stage_db_config|
    stage_db_config.vm.network "private_network", ip: "192.168.56.210"
    stage_db_config.vm.synced_folder ".", "/vagrant", disabled: true
    stage_db_config.vm.provider :virtualbox do |virtualbox|
      virtualbox.customize ["modifyvm", :id, "--name", "dbserver1"]
      virtualbox.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      virtualbox.customize ["modifyvm", :id, "--memory", "512"]
    end
    stage_db_config.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/playbook.yml"
      ansible.inventory_path = "./provisioning/inventory_staging"
    end
  end

  
end