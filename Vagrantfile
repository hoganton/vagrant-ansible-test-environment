# -*- mode: ruby -*-
# vi: set ft=ruby :

# OS Images / Vagrant Boxes
UBUNTU_14_IMAGE = "ubuntu/trusty64";
UBUNTU_16_IMAGE = "aspyatkin/ubuntu-16.04-server-amd64";
UBUNTU_18_IMAGE = "peru/ubuntu-18.04-server-amd64";
CENTOS_6_IMAGE = "generic/centos6";
CENTOS_7_IMAGE = "generic/centos7";

# OS Version / Vagrant Box version
UBUNTU_14_VERSION = "20190402.0.0";
UBUNTU_16_VERSION = "3.1.1";
UBUNTU_18_VERSION = "20190401.01";
CENTOS_6_VERSION = "1.9.20";
CENTOS_7_VERSION = "1.9.20";

# configuration ov the VMs
Vagrant.configure("2") do |config|

    # at-ubuntu-14-node configuration
    config.vm.define "at-ubuntu-14-node" do |subconfig|
        subconfig.vm.box = UBUNTU_14_IMAGE
        subconfig.vm.box_version = UBUNTU_14_VERSION
        subconfig.vm.hostname = "at-ubuntu-14-node"
        subconfig.vm.network :private_network, ip: "10.0.0.11"
        subconfig.vm.provision :hosts, :sync_hosts => true
        subconfig.vm.provision "shell", privileged: true, inline: <<-EOC
            sed -i -e "\\,PasswordAuthentication no, s,PasswordAuthentication no,PasswordAuthentication yes,g" /etc/ssh/sshd_config
            service ssh restart
        EOC
    end

    # at-ubuntu-16-node configuration
    config.vm.define "at-ubuntu-16-node" do |subconfig|
        subconfig.vm.box = UBUNTU_16_IMAGE
        subconfig.vm.box_version = UBUNTU_16_VERSION
        subconfig.vm.hostname = "at-ubuntu-16-node"
        subconfig.vm.network :private_network, ip: "10.0.0.12"
        subconfig.vm.provision :hosts, :sync_hosts => true
        subconfig.vm.provision "shell", privileged: true, inline: <<-EOC
            sed -i -e "\\,PasswordAuthentication no, s,PasswordAuthentication no,PasswordAuthentication yes,g" /etc/ssh/sshd_config
            service ssh restart
	        apt update
            apt install python -y
        EOC
    end

    # at-ubuntu-18-node configuration
    config.vm.define "at-ubuntu-18-node" do |subconfig|
        subconfig.vm.box = UBUNTU_18_IMAGE
        subconfig.vm.box_version = UBUNTU_18_VERSION
        subconfig.vm.hostname = "at-ubuntu-18-node"
        subconfig.vm.network :private_network, ip: "10.0.0.13"
        subconfig.vm.provision :hosts, :sync_hosts => true
        subconfig.vm.provision "shell", privileged: true, inline: <<-EOC
            sed -i -e "\\,PasswordAuthentication no, s,PasswordAuthentication no,PasswordAuthentication yes,g" /etc/ssh/sshd_config
            service ssh restart
	        apt update
            apt install python -y
        EOC

        subconfig.vm.provider "virtualbox" do |vb|
            vb.gui = false
            vb.memory = "512"
        end
    end

    # at-centos-6-node configuration
    config.vm.define "at-centos-6-node" do |subconfig|
        subconfig.vm.box = CENTOS_6_IMAGE
        subconfig.vm.box_version = CENTOS_6_VERSION
        subconfig.vm.hostname = "at-centos-6-node"
        subconfig.vm.network :private_network, ip: "10.0.0.14"
        subconfig.vm.provision :hosts, :sync_hosts => true
        subconfig.vm.provision "shell", privileged: true, inline: <<-EOC
            sed -i -e "\\,PasswordAuthentication no, s,PasswordAuthentication no,PasswordAuthentication yes,g" /etc/ssh/sshd_config
            service sshd restart
	        yum check-update
            yum install libselinux-python -y
        EOC
    end

    # at-centos-7-node configuration
    config.vm.define "at-centos-7-node" do |subconfig|
        subconfig.vm.box = CENTOS_7_IMAGE
        subconfig.vm.box_version = CENTOS_7_VERSION
        subconfig.vm.hostname = "at-centos-7-node"
        subconfig.vm.network :private_network, ip: "10.0.0.15"
        subconfig.vm.provision :hosts, :sync_hosts => true
        subconfig.vm.provision "shell", privileged: true, inline: <<-EOC
            sed -i -e "\\,PasswordAuthentication no, s,PasswordAuthentication no,PasswordAuthentication yes,g" /etc/ssh/sshd_config
            service sshd restart
	        yum check-update
            yum install libselinux-python -y
        EOC
    end

    # at-ansible-master configuration (should always be the last one)
    config.vm.define "at-ansible-master" do |subconfig|
        subconfig.vm.box = UBUNTU_18_IMAGE
        subconfig.vm.box_version = UBUNTU_18_VERSION
        subconfig.vm.hostname = "at-ansible-master"
        subconfig.vm.network :private_network, ip: "10.0.0.10"
        subconfig.vm.synced_folder "../", "/opt/ansible-test"
        subconfig.vm.provision "file", source: "./ssh_keys/id_rsa", destination: "/home/vagrant/.ssh/"
        subconfig.vm.provision "file", source: "./ssh_keys/id_rsa.pub", destination: "/home/vagrant/.ssh/"
        subconfig.vm.provision :hosts, :sync_hosts => true

        subconfig.vm.provision "shell", privileged: true, inline: <<-EOC
	        export DEBIAN_FRONTEND=noninteractive
            apt update
            apt install software-properties-common git ansible sshpass python-apt -y
        EOC

        subconfig.vm.provision "shell", inline: <<-EOC
            chmod 600 /home/vagrant/.ssh/id_rsa
            chmod 644 /home/vagrant/.ssh/id_rsa.pub
            mkdir /home/vagrant/.ssh/controlmasters
            echo export ANSIBLE_CONFIG=/opt/ansible-test/test_environment/test-ansible.cfg >> /home/vagrant/.bashrc
            echo cd /opt/ansible-test/ >> /home/vagrant/.bashrc
            ANSIBLE_CONFIG=/opt/ansible-test/test_environment/test-ansible.cfg ansible-playbook /opt/ansible-test/site.yml -c local --limit at-ansible-master --tags ansible_test_env_setup
        EOC

        subconfig.vm.provider "virtualbox" do |vb|
            vb.gui = false
            vb.memory = "512"
        end
    end
end
