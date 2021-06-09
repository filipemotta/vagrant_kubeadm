# -*- mode: ruby -*-
# vi:set ft=ruby sw=2 ts=2 sts=2:

NUM_MASTER_NODE = 1
NUM_WORKER_NODE = 2

IP_NW = "192.168.5."
MASTER_IP_START = 10
NODE_IP_START = 20

Vagrant.configure("2") do |config|
  
  config.vm.box = "ubuntu/bionic64"

  config.vm.box_check_update = false


  # Provision Master Nodes
  (1..NUM_MASTER_NODE).each do |i|
      config.vm.define "master" do |node|
        
        node.vm.provider "virtualbox" do |vb|
            vb.name = "kubernetes-master"
            vb.memory = 2048
            vb.cpus = 2
        end
        node.vm.hostname = "master"
        node.vm.network :private_network, ip: IP_NW + "#{MASTER_IP_START + i}"

        node.vm.provision "setup-hosts", :type => "shell", :path => "ubuntu/vagrant/setup-hosts.sh" do |s|
          s.args = ["enp0s8"]
        end

        node.vm.provision "setup-dns", type: "shell", :path => "ubuntu/update-dns.sh"
        node.vm.provision "install-docker", type: "shell", :path => "ubuntu/install-docker.sh"
        node.vm.provision "allow-bridge-nf-traffic", :type => "shell", :path => "ubuntu/allow-bridge-nf-traffic.sh"

      end
  end

  # Provision Worker Nodes
  (1..NUM_WORKER_NODE).each do |i|
    config.vm.define "worker-#{i}" do |node|
        node.vm.provider "virtualbox" do |vb|
            vb.name = "kubernetes-worker-#{i}"
            vb.memory = 2048
            vb.cpus = 1
        end
        node.vm.hostname = "worker-#{i}"
        node.vm.network :private_network, ip: IP_NW + "#{NODE_IP_START + i}"

        node.vm.provision "setup-hosts", :type => "shell", :path => "ubuntu/vagrant/setup-hosts.sh" do |s|
          s.args = ["enp0s8"]
        end

        node.vm.provision "setup-dns", type: "shell", :path => "ubuntu/update-dns.sh"
        node.vm.provision "install-docker", type: "shell", :path => "ubuntu/install-docker.sh"
        node.vm.provision "allow-bridge-nf-traffic", :type => "shell", :path => "ubuntu/allow-bridge-nf-traffic.sh"

    end
  end
end
