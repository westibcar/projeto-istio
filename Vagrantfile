# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Definições globais
  config.vm.box = "ubuntu/focal64"
  
  # Máquina Master (Control Plane)
  config.vm.define "k8s-master" do |master|
    master.vm.hostname = "k8s-master"
    master.vm.network "private_network", type: "dhcp"
    master.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.cpus = 2
    end
    master.vm.provision "shell", inline: <<-SHELL
      # Instalação do Kubernetes e kubeadm
      sudo apt-get update
      sudo apt-get install -y apt-transport-https ca-certificates curl
      curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
      sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
      sudo apt-get update
      sudo apt-get install -y kubelet kubeadm kubectl docker.io
      sudo systemctl enable kubelet
      sudo systemctl enable docker
      sudo kubeadm init --pod-network-cidr=10.244.0.0/16
      
      # Configuração do kubectl para o usuário vagrant
      mkdir -p /home/vagrant/.kube
      sudo cp -i /etc/kubernetes/admin.conf /home/vagrant/.kube/config
      sudo chown vagrant:vagrant /home/vagrant/.kube/config
      
      # Aplicação da rede Flannel
      kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
    SHELL
  end

  # Máquina Worker 1
  config.vm.define "k8s-worker1" do |worker1|
    worker1.vm.hostname = "k8s-worker1"
    worker1.vm.network "private_network", type: "dhcp"
    worker1.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 2
    end
    worker1.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y apt-transport-https ca-certificates curl
      curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
      sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
      sudo apt-get update
      sudo apt-get install -y kubelet kubeadm kubectl docker.io
      sudo systemctl enable kubelet
      sudo systemctl enable docker
    SHELL
  end

  # Máquina Worker 2
  config.vm.define "k8s-worker2" do |worker2|
    worker2.vm.hostname = "k8s-worker2"
    worker2.vm.network "private_network", type: "dhcp"
    worker2.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 2
    end
    worker2.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y apt-transport-https ca-certificates curl
      curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
      sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
      sudo apt-get update
      sudo apt-get install -y kubelet kubeadm kubectl docker.io
      sudo systemctl enable kubelet
      sudo systemctl enable docker
    SHELL
  end
end
