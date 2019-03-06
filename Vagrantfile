# -*- mode: ruby -*-
# vi: set ft=ruby :

# """
# Provisioning may take a long time, do not stop it or suspend it earlier, it
# can cause unexpected behavior.
#
#
# Usage:
#  * vagrant up              -- deploy and provision
#  * vagrant up <node name>  -- deploy and provision specified node
#  * vagrant halt            -- stop the running guest
#  * vagrant destroy         -- destroy the deployed guest
#  * vagrant -h              -- show common vagrant commands
# """


# """
# All Vagrant configuration is done below.
#
# The "2" in Vagrant.configure configures the configuration version (Vagrant
# support older styles for backward compatibility). Please don't change it
# unless you know what you're doing.
# """
Vagrant.configure("2") do |config|
  # For a complete reference of configuration, please see the online documentation at https://docs.vagrantup.com

  # Config part
  hostname = "data.analysis"
  ip_address = "192.168.10.10"
  memory = 8192
  number_of_cpu = 4

  # Orchestration
  config.vm.define "data.analysis" do |core|
    core.vm.hostname = "data.analysis"
    core.vm.box = "ubuntu/bionic64"
    core.vm.network :private_network, ip: ip_address, netmask: "255.255.255.0"
    config.vm.network "forwarded_port", guest: 22, host: 2222
    core.vm.synced_folder "./", "/vagrant", owner: "vagrant", group: "vagrant"
    core.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--name", hostname]
      v.customize ["modifyvm", :id, "--memory", memory]
      v.customize ["modifyvm", :id, "--cpus", number_of_cpu]
      v.customize ["modifyvm", :id, "--uartmode1", "disconnected"]
    end

    # pro
    core.vm.provision :ansible_local do |ansible|
      ansible.install_mode = "pip"  # Use the latest Ansible
      ansible.become = true
      ansible.playbook = "/vagrant/provisioning/provision.yml"
      ansible.verbose = false
    end
  end
end