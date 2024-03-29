# Openstack Lab
Vagrant generated Openstack. A single VM installation on Centos 7, using Packstack ([Packstack install guide](https://www.rdoproject.org/install/packstack/)).

Default machine configuration:
- Centos 7 64 bits
- 4GB RAM
- 2 vCPUs
## Usage
### Start
To create and launch the VM simply execute:
```
$ vagrant up
```
Openstack installation and configuration will take a while.
### Stop
```
$ vagrant halt
```
To stop and detele the VM:
```
$ vagrant destroy -f
```
### Restart
```
$ vagrant reload
```
### Machine configuration
You can adjust VM specifications simply modifying the Vagrantfile:
- vb.memory: VM's RAM in KB (default 4096)
- vb.cpus: VM's cpus number (default 2)
- Openstack Dashboard port: Guest's 80 port is forwarded by default to host's 8088 port by default. You can change the mapped port editing the line
```
config.vm.network "forwarded_port", guest: 80, host: 8088
```
- VM's private network can be modified editing this line:
```
config.vm.network "private_network", ip: "10.0.0.10"
```
### Accessing to Openstack Dashboard
Once the VM is created and Openstack installed and configured, the Dashboard is reachable at: http://localhost:8088/dashboard.

A restart could be required after the VM creation to be able to access to Dashboard.

Dashboard's default username and password are generated by the installer and stored in /root/keystoncerc_admin. You can connect to the VM and sudo cat the file:
```
$ vagrant ssh
[vagrant@openstack-lab ~]$ sudo cat /root/keystoncerc_admin
```