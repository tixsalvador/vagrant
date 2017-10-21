# -*- mode: ruby -*-
# vi: set ft=ruby :

nodes = {
 'controller' => [1,11],
 }
 
Vagrant.configure("2") do |config|
  config.vm.box = "redhat74"
  config.vm.box_url = "file:///d:/Users/edge/VirtualBox VMs\vagrant\boxes/redhat74.box"
  config.vm.synced_folder ".", "/vagrant", type: "nfs"
  config.vm.usable_port_range = 2800..2900  
  
  nodes.each do |prefix, (count, ip_start)|
    count.times do |i|
	  hostname = "%s" % [prefix, (i+1)]
	  
	  config.vm.define "#{hostname}" do |box|
	    box.vm.hostname = "#{hostname}"
		box.vm.network :private_network, ip: "10.0.0.#{ip_start+i}", :netmask => "255.255.255.0"
		if prefix == "controller" or prefix == "compute"
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
		  end #if
		end # box.vm.provider  
	  end # config.vm.define
	end # count.times
  end #nodes.each
end #Vagrant.configure  
