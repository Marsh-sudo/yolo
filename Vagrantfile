# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "geerlingguy/ubuntu2004"
  config.vm.network :private_network, ip: "192.168.47.7"
  config.vm.hostname = "drupal.test"
  config.ssh.insert_key = false

  config.vm.provider :virtualbox do |v|
    v.memory = 2048
  end

  # Ansible provisioning.
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
  end
end