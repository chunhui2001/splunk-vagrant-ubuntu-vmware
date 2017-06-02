# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

splunk_deb = "splunk-6.6.0-1c4f3bbe1aea-linux-2.6-amd64.deb"
host_name = "splunk-standalone"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Use Ubuntu 16.04 VM box
  config.vm.box = "bento/ubuntu-16.04"

  # Create private network with IP based on DHCP
  # Forward Splunk Web port
  # Forward Splunk MGMT port
  config.vm.network "private_network", type: "dhcp"
  config.vm.network "forwarded_port", guest: 8000, host: 8000
  config.vm.network "forwarded_port", guest: 8089, host: 8089

  # Sync sw folder to /media/rpms
  config.vm.synced_folder "sw/", "/media/debs"

  # Set VM mem and num VCPU settings
  config.vm.provider "vmware_fusion" do |v|
    v.vmx["memsize"] = "2048"
    v.vmx["numvcpus"] = "2"
  end

  # Update Linux
  # Install Splunk from the synced folder
  # Start Splunk with accepted license
  # Enable Splunk boot-start
  config.vm.provision "shell", inline: "sudo apt-get update"
  config.vm.provision "shell", inline: "sudo dpkg -i /media/debs/splunk-6.6.0-1c4f3bbe1aea-linux-2.6-amd64.deb"
  config.vm.provision "shell", inline: "sudo /opt/splunk/bin/splunk start --accept-license --answer-yes"
  config.vm.provision "shell", inline: "sudo /opt/splunk/bin/splunk enable boot-start"
end
