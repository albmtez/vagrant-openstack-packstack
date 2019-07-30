# -*- mode: ruby -*-
# vi: set ft=ruby :

################################################################################
# Openstack
#
# Installed in Centos 7 using Packstack.
#
# Install guide: https://www.rdoproject.org/install/packstack/
################################################################################

# VM memory.
vm_memory = 6144
# VM number of cpus.
vm_cpus = 2

# Script to provision the machine:
# - Packages update
# - Disable firewall and Network Manager
# - Install Openstack Queens reposiroty
# - Install Packstack repository
# - Install Openstack all components in one node
$provisioning_script = <<SCRIPT
yum update -y

echo "LANG=en_US.utf-8" >> /etc/environment
echo "LC_ALL=en_US.utf-8" >> /etc/environment

systemctl disable firewalld
systemctl stop firewalld
systemctl disable NetworkManager
systemctl stop NetworkManager
systemctl enable network
systemctl start network

yum update -y
yum install -y centos-release-openstack-stein
yum update -y
yum install -y openstack-packstack

packstack --allinone
SCRIPT

Vagrant.configure("2") do |config|

	config.vm.define "OpenstackPackstack" do |openstack|
		openstack.vm.box = "centos/7"
		openstack.vm.hostname = "openstack-packstack"
		openstack.vm.network "private_network", ip: "10.0.0.100"
		openstack.vm.network "forwarded_port", guest:80, host: 8088
		openstack.ssh.insert_key = false

		openstack.vm.provision "shell", inline: $provisioning_script

		openstack.vm.provider :virtualbox do |vb|
			vb.gui = false
			vb.name = "OpenstackPackstack"
			vb.memory = "#{vm_memory}"
			vb.cpus = "#{vm_cpus}"
		end

		openstack.vm.provider :libvirt do |libvirt|
			libvirt.default_prefix = "vagrant"
			libvirt.memory = "#{vm_memory}"
			libvirt.cpus = "#{vm_cpus}"
		end
	end
end
