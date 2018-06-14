# -*- mode: ruby -*-
# vi: set ft=ruby :

ADDRESS="10.0.0.11"
MEMORY="512"

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/xenial64"

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", MEMORY]
  end

  config.vm.define :dev do |node|
    node.vm.network :private_network, ip: "#{ADDRESS}"
    node.vm.network :forwarded_port, guest: 80, host: 8080

    # Install Python for Ansible
    config.vm.provision "shell", inline: "sudo apt-get update && sudo apt-get install -y python-minimal"

    node.vm.provision "ansible" do |ansible|
      ansible.playbook = "site.yml"
      ansible.extra_vars = { ansible_ssh_user: 'vagrant', display_skipped_hosts: 'False' }
      ansible.become = true
    end 
  end

end
