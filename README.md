# gls-jenkins

## Introduction

This project is a way to automate the installation of Jenkins instance on a local VM to customize the way Global Learning Services (GLS) automates some tasks related to course release process. The automation is based on Vagrant and Ansible. 

## Creating the environment

### Install base tools

- Vagrant: https://www.vagrantup.com/
- VirtualBox: https://www.virtualbox.org/wiki/Downloads

### Deploy a custom RHEL7.5 Vagrant box locally

Download the box from : https://drive.google.com/open?id=1hyRVCDkDQLdg1vdzLxlDbUCBVW-gJvXa

Run the following command to install the box locally:

 `vagrant box add gls rhel-7.5.virtualbox.box`

### Install Vagrant Registration plugin.

 Configure the vagrant registration plugin (to register the box with `subscription-manager`):

 `vagrant plugin install vagrant-registration`
 
 ### Configure the registration plugin

 To authenticate the box, you need to provide your RHN ID and password in a configuration file.

 Copy the `vagrant.d.Vagrantfile` file to `~/.vagrant.d` using the following command:

`cp vagrant.d.Vagrantfile ~/vagrant.d/Vagrantfile`

 Update the file with your credentials:

[NOTE]
====
 Vagrant.configure("2") do |config|
   config.registration.username = 'RHNID'
   config.registration.password = 'RHNID-Password'
end 
====

### Deploy Jenkins locally

Run the following command from the root directory of this project:

`vagrant up`

[NOTE]
====
The installation uses the following module from ansible-galaxy:

geerlingguy.jenkins

There is a problem with this module that does not allow you installing the plugins during the first run `https://github.com/geerlingguy/ansible-role-jenkins/issues/213`

After the first failed execution, run the following commands:
`vagrant halt`
`vagrant up --provision`
====