# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/bionic64"
  config.vm.box_check_update = false

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    vb.customize ["modifyvm", :id, "--audio", "none"]
  end

  (0..2).each do |n|
    config.vm.define "controller-#{n}" do |c|
        c.vm.hostname = "controller-#{n}"
        c.vm.network "private_network", ip: "192.168.198.1#{n}"
        c.vm.provision :shell, :path => "scripts/cp-controller", :args => "#{n}"
        c.vm.provision :shell, :path => "scripts/install-etcd", :args => "controller-#{n} 192.168.198.1#{n}"
        c.vm.provision :shell, :path => "scripts/install-kube-control-plane", :args => "192.168.198.1#{n}"
        c.vm.provider "virtualbox" do |vb|
          vb.memory = "1024"
      end
    end
  end

  (0..2).each do |n|
    config.vm.define "worker-#{n}" do |c|
        c.vm.hostname = "worker-#{n}"
        c.vm.network "private_network", ip: "192.168.198.2#{n}"
        c.vm.provision :shell, :path => "scripts/cp-worker", :args => "#{n}"
        #c.vm.provision :shell, :path => "scripts/vagrant-setup-hosts-file.bash"
        c.vm.provider "virtualbox" do |vb|
          vb.memory = "1024"
      end
    end
  end

  config.vm.define "kube-api-lb" do |c|
    c.vm.hostname = "kube-api-lb"
    c.vm.network "private_network", ip: "192.168.198.40"
    c.vm.provision :shell, :path => "scripts/vagrant-lb"
    c.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
    end
  end

end
