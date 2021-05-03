# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
    config.vm.define "nginx" do |nginx|
        nginx.vm.hostname = "nginx.example.com"
        nginx.vm.box = "generic/centos7"
        nginx.vm.network "forwarded_port", guest: 80, host:8080, host_ip: "127.0.0.1"
        nginx.vm.network "private_network", ip: "10.10.10.11"
        nginx.vm.provision "shell", inline: <<-SHELL
            sudo yum install epel-release -y
            sudo yum install nginx -y
            sudo systemctl --now enable nginx 
            sudo firewall-cmd --permanent --zone=public --add-service=http 
            sudo firewall-cmd --permanent --zone=public --add-service=https
            sudo firewall-cmd --reload
            sudo firewall-cmd --permanent --zone=public --add-port=8080
        SHELL
        nginx.vm.provider "virtualbox" do |vb|
            vb.cpus = "1"
            vb.memory = "1024"
            vb.customize ["modifyvm", :id, "--groups", "/vagranting"]
            vb.name = "nginx"
        end
    end
end

