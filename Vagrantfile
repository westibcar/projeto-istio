# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Definições globais
  config.vm.box = "ubuntu/focal64"
  config.vm.synced_folder '.', '/vagrant', disabled: true
  
  # Máquina controlplane (Control Plane)
  config.vm.define "k8s-controlplane" do |controlplane|
    controlplane.vm.hostname = "k8s-controlplane"
    controlplane.vm.network "private_network", ip: "192.168.56.10", auto_config: false
    controlplane.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.cpus = 2
    end
    controlplane.vm.provision "shell", inline: <<-SHELL
      # Instalação do Kubernetes e kubeadm
      sudo apt-get update
      sudo apt install containerd -y
      sudo systemctl enable --now containerd
      sudo apt-get install -y apt-transport-https ca-certificates curl gpg
      sudo mkdir -p -m 755 /etc/apt/keyrings
      curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
      echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
      cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.ipv4.ip_forward = 1
EOF

      # Atualiza o índice do apt, instala kubelet, kubeadm e kubectl, e fixa a versão:
      sudo apt-get update
      sudo apt-get install -y kubelet kubeadm kubectl
      sudo apt-mark hold kubelet kubeadm kubectl
      sudo systemctl enable --now kubelet
      sudo kubeadm init --pod-network-cidr=10.244.0.0/16 
    
      # Configuração do kubectl para o usuário vagrant
      mkdir -p /home/vagrant/.kube
      sudo cp -i /etc/kubernetes/admin.conf /home/vagrant/.kube/config
      sudo chown vagrant:vagrant /home/vagrant/.kube/config
      
      # Aplicação da rede Flannel
      kubectl apply -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml
    SHELL
  end

  # Máquina Worker 1
  config.vm.define "k8s-node01" do |node01|
    node01.vm.hostname = "k8s-node01"
    node01.vm.network "private_network", ip: "192.168.56.11", auto_config: false
    node01.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 2
    end
    node01.vm.provision "shell", inline: <<-SHELL
      # Instalação do Kubernetes e kubeadm
      sudo apt-get update
      sudo apt install containerd -y
      sudo systemctl enable --now containerd
      sudo apt-get install -y apt-transport-https ca-certificates curl gpg
      sudo mkdir -p -m 755 /etc/apt/keyrings
      curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
      echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
      cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.ipv4.ip_forward = 1
EOF

      # Atualiza o índice do apt, instala kubelet, kubeadm e kubectl, e fixa a versão:
      sudo apt-get update
      sudo apt-get install -y kubelet kubeadm kubectl
      sudo apt-mark hold kubelet kubeadm kubectl
      sudo systemctl enable --now kubelet
    SHELL
  end

  # Máquina Worker 2
  config.vm.define "k8s-node02" do |node02|
    node02.vm.hostname = "k8s-node02"
    node02.vm.network "private_network", ip: "192.168.56.12", auto_config: false
    node02.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 2
    end
    node02.vm.provision "shell", inline: <<-SHELL
      # Instalação do Kubernetes e kubeadm
      sudo apt-get update
      sudo apt install containerd -y
      sudo systemctl enable --now containerd
      sudo apt-get install -y apt-transport-https ca-certificates curl gpg
      sudo mkdir -p -m 755 /etc/apt/keyrings
      curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
      echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
      cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.ipv4.ip_forward = 1
EOF

      # Atualiza o índice do apt, instala kubelet, kubeadm e kubectl, e fixa a versão:
      sudo apt-get update
      sudo apt-get install -y kubelet kubeadm kubectl
      sudo apt-mark hold kubelet kubeadm kubectl
      sudo systemctl enable --now kubelet 
    SHELL
  end
end