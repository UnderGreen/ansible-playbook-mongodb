# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
	config.vm.box = "ubuntu/trusty64"
	# workaround for https://github.com/mitchellh/vagrant/issues/5048
	config.ssh.insert_key = false

	# General VirtualBox VM configuration.
	config.vm.provider :virtualbox do |v|
		v.customize ["modifyvm", :id, "--memory", 512]
		v.customize ["modifyvm", :id, "--cpus", 1]
		v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
		v.customize ["modifyvm", :id, "--ioapic", "on"]
	end

	if Vagrant.has_plugin?("vagrant-cachier")
		config.cache.scope = :machine
	end

	config.vm.define "mongodb1" do |node|
		node.vm.hostname = "mongodb1.dev"
		node.vm.network :private_network, ip: "192.168.56.4"
	end

	config.vm.define "mongodb2" do |node|
		node.vm.hostname = "mongodb2.dev"
		node.vm.network :private_network, ip: "192.168.56.5"
	end

	config.vm.define "arbiter" do |node|
		node.vm.hostname = "arbiter.dev"
		node.vm.network :private_network, ip: "192.168.56.6"
	end

	# single mongodb instance without auth
	config.vm.define "mongodb-wa" do |node|
		node.vm.hostname = "mongodb-wa.dev"
		node.vm.network :private_network, ip: "192.168.56.7"
		# Run Ansible provisioner once for all VMs at the end.
		node.vm.provision "ansible" do |ansible|
			ansible.playbook = "deploy.yml"
			ansible.limit = "all"
			ansible.sudo = true
			ansible.groups = {
				"mongodb_master" => ['mongodb1'],
				"mongodb_replicas" => ['mongodb2', 'arbiter'],
				"mongodb_wa" => ['mongodb-wa'],
				"mongodb_rep:children" => ['mongodb_master', 'mongodb_replicas'],
				"mongodb:children" => ['mongodb_rep', 'mongodb_wa']
			}
			ansible.extra_vars = {
				ansible_ssh_user: 'vagrant',
				ansible_ssh_private_key_file: "~/.vagrant.d/insecure_private_key"
			}
		end
	end

end