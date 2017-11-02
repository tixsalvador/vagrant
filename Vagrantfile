# -*- mode: ruby -*-
# vi: set ft=ruby :

os = "centos"
#os = "redhat"

nodes = {
 'controller' => [1,11],
 'compute' => [1,31],
 'controlmanager' => [1,61],
 }
 
Vagrant.configure("2") do |config|
  if os == "centos"
    config.vm.box = "centos-7"
    config.vm.box_url = "file:///d:/Users/edge/boxes/centos7.box"
  elsif os == "redhat"
    config.vm.box = "redhat-7"
    config.vm.box_url = "file:///d:/Users/edge/boxes/redhat74.box"
  end #if os
  config.vm.synced_folder ".", "/vagrant", type: "nfs"
  config.vm.usable_port_range = 2800..2900  
  
  nodes.each do |prefix, (count, ip_start)|
    count.times do |i|
	  hostname = "%s" % [prefix, (i+1)]
	  
	  config.vm.define "#{hostname}" do |box|
	    box.vm.hostname = "#{hostname}"
		box.vm.network :private_network, ip: "10.0.0.#{ip_start+i}", :netmask => "255.255.255.0", :gateway => "10.0.0.1"
		if prefix == "controller" or prefix == "compute" or prefix == "controlmanager"
		  box.vm.network "public_network"
		end #if
		
		box.vm.provider :virtualbox do |vb|
		  vb.customize ["modifyvm", :id, "--memory", 1024]
		  vb.customize ["modifyvm", :id, "--cpus", 1]
		  vb.customize ["modifyvm", :id, "--nicpromisc3", "allow-all"]
		  vb.customize ["modifyvm", :id, "--nicpromisc4", "allow-all"]
		  if prefix == "controller" 
		    vb.customize ["modifyvm", :id, "--memory", 2048]
			vb.customize ["modifyvm", :id, "--cpus", 2]
		  end #if memory
		end # box.vm.provider  
	  end # config.vm.define
	end # count.times
  end #nodes.each
end #Vagrant.configure  
