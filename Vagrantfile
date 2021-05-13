# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
    config.vm.define "nginx" do |nginx|
        nginx.vm.hostname = "nginx.example.com"
        nginx.vm.box = "generic/centos7"
        nginx.vm.network "forwarded_port", guest: 80, host:8080, host_ip: "127.0.0.1"
        nginx.vm.network "private_network", ip: "10.10.10.11"
        nginx.vm.synced_folder ".", "/vagrant", type: "rsync"
        nginx.vm.provision "shell", inline: <<-SHELL
            sudo yum install epel-release -y
            sudo yum install nginx -y
            sudo systemctl --now enable nginx 
            sudo firewall-cmd --permanent --zone=public --add-service=http 
            sudo firewall-cmd --permanent --zone=public --add-service=https
            sudo firewall-cmd --reload
            # Setup DNS client with vagranting-dns
            ETH0=$(sudo nmcli connection show | grep eth0 | cut -d ' ' -f 4)
            ETH1=$(sudo nmcli connection show | grep eth1 | cut -d ' ' -f 4)
            sudo nmcli connection modify $ETH1 ipv4.dns 10.10.10.2
            sudo nmcli connection modify $ETH1 ipv4.dns-search example.com
            sudo nmcli connection modify $ETH1 ipv4.dns-priority 1
            sudo nmcli connection modify $ETH0 ipv4.dns-priority 10
            sudo nmcli connection up $ETH1
            sudo nmcli connection up $ETH0
        SHELL
        nginx.vm.provider "virtualbox" do |vb|
            vb.cpus = "1"
            vb.memory = "512"
            vb.customize ["modifyvm", :id, "--groups", "/vagranting"] #If you have problems with folder exist error -> Comment out this line
            vb.name = "nginx"
        end
    end
end

