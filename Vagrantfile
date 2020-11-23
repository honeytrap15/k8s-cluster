# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.hostmanager.enabled = true

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end

  ##########
  # k8s master config
  ##########
  config.vm.define "master", primary: true do |master|

    master.vm.box = "honeytrap15/centos77-k8s-base"
    master.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
      vb.cpus = 2
    end

    # configure network
    master.vm.hostname = "master"
    master.vm.network "private_network", ip: "192.168.33.10"

    # init k8s for master
    master.vm.provision "shell", inline: <<-SHELL
      kubeadm init --apiserver-advertise-address=192.168.33.10 --pod-network-cidr=10.244.0.0/16

      mkdir -p $HOME/.kube
      cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
      chown $(id -u):$(id -g) $HOME/.kube/config

      mkdir -p /home/vagrant/.kube
      cp -i /etc/kubernetes/admin.conf /home/vagrant/.kube/config
      chown -R vagrant:vagrant /home/vagrant/.kube/config

      kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
    SHELL

  end

  ##########
  # k8s worker config
  ##########
  config.vm.define "worker", primary: true do |worker|

    worker.vm.box = "honeytrap15/centos77-k8s-base"
    worker.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
      vb.cpus = 2
    end

    # configure network
    worker.vm.hostname = "worker"
    worker.vm.network "private_network", ip: "192.168.33.11"

  end

  ##########
  # k8s router config
  ##########
  config.vm.define "router", primary: true do |router|

    router.vm.box = "honeytrap15/centos77-k8s-base"
    router.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
      vb.cpus = 2
    end

    # configure network
    router.vm.hostname = "router"
    router.vm.network "private_network", ip: "192.168.33.12"

  end

end
