# vagrant-ansible-test-environment

## Installation
Download Vagrant from following URL:
https://www.vagrantup.com/downloads.html

Install it and also download and install VirtualBox from:
https://www.virtualbox.org/wiki/Downloads

## Installation
You also need to install some plugins for vagrant to work. Just open the Windows Powershell and run the following commands:
`vagrant plugin install vagrant-hosts`

If you use Visual Studio Code, it is recommended to install the Ruby extension.

## Starting Vagrant machines
To start the vagrant machines just simply open a command prompt (cmd, git bash, powershell etc.) inside the directory of the Vagrantfile and execute following command and go grab a coffee:
`vagrant up`

Warnings!!
- The initial creation of the ansible-master can take some time. It is recommended to do some other work during that initial installation.
- You might have to "allow" Virtualbox to create networks as Administrator. Keep your eyes peeled for popups.

## Used Vagrant Boxes in the test environment

| Vagrant Box                          | Version      | OS           | URL for more Info                                                       |
| ------------------------------------ |:------------:|:------------:| -----------------------------------------------------------------------:|
| ubuntu/trusty64                      | 20190402.0.0 | Ubuntu 14.04 | https://app.vagrantup.com/bento/boxes/ubuntu-14.04/versions/201808.24.0 |
| aspyatkin/ubuntu-16.04-server-amd64" | 3.1.1        | Ubuntu 16.04 | https://app.vagrantup.com/bento/boxes/ubuntu-16.04/versions/201812.27.0 |
| peru/ubuntu-18.04-server-amd64       | 20190401.01  | Ubuntu 18.04 | https://app.vagrantup.com/bento/boxes/ubuntu-18.04/versions/201812.27.0 |
| generic/centos6                      | 1.9.20       | CentOS 6.10  | https://app.vagrantup.com/bento/boxes/centos-6.10                       |
| generic/centos6                      | 1.9.20       | CentOS 7.6   | https://app.vagrantup.com/bento/boxes/centos-7.6                        |

## Hyper-V and VirtualBox compatibility
Starting from VirtualBox 6 VirtualBox can run in parallel to Hyper-V though VirtualBox VMs will be slower as they will not be able to leverage virtualization features.

On a Windows **1809** and **1903** build one needs to set an additional configuration to make VirtualBox working fine together with Hyper-V again. (https://forums.virtualbox.org/viewtopic.php?f=6&t=90853&start=120)
`.\VBoxManage.exe setextradata global "VBoxInternal/NEM/UseRing0Runloop" "0"`

Info: There might be an issue with VirtualBox **6.0.10** where Linux Virtual Machines freeze on `vagrant up`. The workaround for this problem is to simply right click the frozen VM on VirtualBox and click "Reset" after that the Box should start normally and vagrant proceeds automatically.

## Useful Vagrant commands
```bash
vagrant status						# shows the  status of all vagrant machines which are part of the vagrantfile
vagrant up							# starts all vagrant machines
vagrant up <vagrant_machine>		# starts a specific vagrant machine
vagrant hold						# stops all vagrant machines gracefully
vagrant hold <vagrant_machine>		# stops a specific machine gracefully
vagrant destroy						# removes all vagrant machines
vagrant destroy <vagrant_machine>	# removes a specific vagrant machine
vagrant ssh <vagrant_machine>		# connect to a specific vagrant machine via ssh
```
