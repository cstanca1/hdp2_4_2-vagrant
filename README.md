# Setup Hortonworks Data Platform using Vagrant, VirtualBox and Ambari

## Objective
Deploy a 4-node HDP 2.4.2 cluster with Apache Ambari 2.2.2, Vagrant and VirtualBox on OS X host. 
This is helpful for development and proof of concepts.

## Scope
This approach has been tested on OS X host, but it should work on all supported Vagrant and VirtualBox environments.

## Pre-requisites
- Minimum 9 GB of RAM for the HDP 2.4.2 cluster

## Steps
1. Download and install Vagrant for your host OS: https://www.vagrantup.com/downloads.html
2. Download and install VirtualBox for your host OS: https://www.virtualbox.org/wiki/Downloads
3. Download and install git client for your host
4. Open a command shell and change to the folder where you plan to clone the github repository
5. Clone the Github repository:  ```git clone https://github.com/cstanca1/hdp2_4_2-vagrant.git```
6. Change directory to hdp_2.4.2-vagrant, the folder that includes Vagrantfile and: ```mkdir data```. This /data folder will be needed for guest VMs to share with the host.

## Start Ambari VM
Vagrant (via Vagrantfile) is configured to use Centos 6.7 as the base box and includes the pre-requisites for installing HDP.
4 VMs will be created: 1 Ambari Server (ambari1), 1 Hadoop master (master1) and 2 slaves (slave1, slave2).

Let's start ambari VM first:

```vagrant up ambari1```

## Install and Setup Ambari Server

### Create a Local Ambari  Repository

```vagrant ssh ambari1```

```sudo su -```

```cd /etc/yum.repos.d```

```wget http://public-repo-1.hortonworks.com/ambari/centos6/2.x/updates/2.2.2.0/ambari.repo```

### SSH Access and Starting the Other Three VMs
Add at the same path with files downloaded from the repoosity, your id_rsa and id_rsa.pub keys (see https://wiki.centos.org/HowTos/Network/SecuringSSH section 7 for instructions on CentOS). You could perform these steps on ambari1 VM and copy these two files to your /vagrant_data folder which shares data between guest and host. Only after you copy those two files, start the other three VMs:

```vagrant up master1```

```vagrant up slave1```

```vagrant up slave2```

### Install Ambari Server

```yum install ambari-server```

### Setup Ambari Server
Run the setup command to configure your Ambari Server, Database, JDK, LDAP, and other options:

```ambari-server setup```

### Start Ambari Server

```ambari-server start```

### Deploy Cluster using Ambari Web UI
Open up a web browser and go to http://ambari1:8080.
Log in with username admin and password admin and follow on-screen instructions, selecting hosts created and services of interest.
