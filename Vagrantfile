# -*- mode: ruby -*-
# vi:set ft=ruby sw=2 ts=2 sts=2:

Vagrant.configure("2") do |config|
  config.vm.provision "shell", inline: <<-SHELL
      apt-get update -y
      echo "10.0.0.10  master-node" >> /etc/hosts
      echo "10.0.0.11  worker-node01" >> /etc/hosts
      echo "10.0.0.12  worker-node02" >> /etc/hosts
      
      cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF
sudo modprobe br_netfilter

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sudo sysctl --system

  SHELL
  
  config.vm.define "master" do |master|
    master.vm.box = "ubuntu/bionic64"
    master.vm.hostname = "master-node"
    master.vm.network "private_network", ip: "10.0.0.10"
    master.vm.provider "virtualbox" do |vb|
        vb.memory = 2048
        vb.cpus = 2
    end
  end

  (1..2).each do |i|

  config.vm.define "node0#{i}" do |node|
    node.vm.box = "ubuntu/bionic64"
    node.vm.hostname = "worker-node0#{i}"
    node.vm.network "private_network", ip: "10.0.0.1#{i}"
    node.vm.provider "virtualbox" do |vb|
        vb.memory = 2048
        vb.cpus = 2
    end
  end
  
  end
end
