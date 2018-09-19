# -*- mode: ruby -*-
# vi: set ft=ruby :

# Fixing issue identified: https://github.com/projectatomic/adb-vagrant-registration/issues/126
module SubscriptionManagerMonkeyPatches
  def self.subscription_manager_registered?(machine)
    true if machine.communicate.sudo("/usr/sbin/subscription-manager list --consumed --pool-only | grep -E '^[a-f0-9]{32}$'")
  rescue
    false
  end
end

VagrantPlugins::Registration::Plugin.guest_capability 'redhat', 'subscription_manager_registered?' do
  SubscriptionManagerMonkeyPatches
end


Vagrant.configure("2") do |config|
  config.vm.box = "gls"
  
  config.vm.network "forwarded_port", guest: 8080, host: 9000
  config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.synced_folder "installers", "/vagrant_data"
#  config.vm.provision "shell", inline: <<-SHELL
#     sudo yum update -y
#     sudo subscription-manager repos --enable rhel-7-server-ansible-2.6-rpms
#     sudo yum install ansible -y
#   SHELL

   config.vm.provision "ansible_local" do |ansible|
     ansible.playbook = "playbook.yml"
     ansible.inventory_path = "hosts"
     ansible.limit = "all"

   end
end
