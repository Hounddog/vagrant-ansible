

Vagrant.configure("2") do |config|
  #Local development environment
  config.vm.define :development do |development_config|
    development_config.vm.box = "ubuntu-precise12042-x64-vbox43"
    development_config.vm.box_url = "https://puphpet.s3.amazonaws.com/ubuntu-precise12042-x64-vbox43.box"

    development_config.vm.network "private_network", ip: "192.168.56.102"


    development_config.vm.synced_folder "./", "/var/www", id: "vagrant-root", :nfs => false

    development_config.vm.usable_port_range = (2200..2250)
    development_config.vm.provider :virtualbox do |virtualbox|
      virtualbox.customize ["modifyvm", :id, "--name", "development"]
      virtualbox.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      virtualbox.customize ["modifyvm", :id, "--memory", "512"]
      virtualbox.customize ["setextradata", :id, "--VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
    end

    development_config.ssh.keep_alive = true
    development_config.vagrant.host = :detect
    config.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/playbook.yml"
    end
  end
end