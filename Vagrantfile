# -*- mode: ruby -*-
# vi: set ft=ruby :

IMAGE_NAME = "marcosx86/buster64"
N = 3

Vagrant.configure("2") do |cfg|
	cfg.ssh.insert_key = false

#	cfg.vm.provider "virtualbox"
#	cfg.vm.provider "virtualbox" do |v|
#		v.memory = 512
#		v.cpus = 2
#	end
	  
	cfg.vm.define "k8s-master" do |master|
		master.vm.provider("virtualbox") do |vbox|
			vbox.memory = 1024
			vbox.cpus = 2
		end
		master.vm.box = IMAGE_NAME
		master.vm.hostname = "k8s-master"
		#master.vm.network "private_network", ip: "192.168.50.10"
		master.vm.network "public_network", bridge: "zt0", ip: "172.25.200.10"
		master.vm.provision("ansible") do |ans|
			ans.playbook = "master-playbook.yaml"
			ans.compatibility_mode = "2.0"
			ans.extra_vars = {
				#node_ip: "192.168.50.10",
				node_ip: "192.168.50.10",
			}
		end
	end

	(1..N).each do |i|
		cfg.vm.define "k8s-node-#{i}" do |node|
			node.vm.provider("virtualbox") do |vbox|
				vbox.memory = 1536
				vbox.cpus = 4
			end
			node.vm.box = IMAGE_NAME
			node.vm.hostname = "k8s-node-#{i}"
			#node.vm.network "private_network", ip: "192.168.50.#{i + 10}"
			node.vm.network "public_network", bridge: "zt0", ip: "172.25.200.#{i + 10}"
			node.vm.provision("ansible") do |ans|
				ans.playbook = "node-playbook.yaml"
				ans.compatibility_mode = "2.0"
				ans.extra_vars = {
					#node_ip: "192.168.50.#{i + 10}",
					node_ip: "172.25.200.#{i + 10}",
				}
			end
		end
	end
end
