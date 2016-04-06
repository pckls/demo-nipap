# -*- mode: ruby -*-
# # vi: set ft=ruby :

# Specify minimum Vagrant version and Vagrant API version
Vagrant.require_version ">= 1.7.0"
VAGRANTFILE_API_VERSION = "2"

# Automatically manage plugins
REQUIRED_PLUGINS = %w( vagrant-auto_network vagrant-hosts vagrant-r10k )
REQUIRED_PLUGINS.each do |plugin|
    system "vagrant plugin install #{plugin}" unless Vagrant.has_plugin? plugin
end

# Do the needful
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

    config.vm.define "demo-nipap" do |node|

        # Provider specific stuff
        node.vm.provider :virtualbox do |vb|
            #vb.cpus = data["cpus"]
            #vb.memory = data["memory"]
            vb.name = "demo-nipap"
        end

        node.vm.box = "puppetlabs/ubuntu-14.04-64-puppet"

        node.vm.hostname = "demo-nipap.example.com"

        node.vm.network :private_network, :auto_network => true
        node.vm.network :forwarded_port, host: "8080", guest: "80"

        node.r10k.puppet_dir = 'puppet'
        node.r10k.puppetfile_path = 'puppet/Puppetfile'
        node.r10k.module_path = 'puppet/environments/vagrant/modules'

        node.vm.provision :puppet do |puppet|
            if File.directory?("puppet/environments/vagrant")
                puppet.environment_path = "puppet/environments"
                puppet.environment = "vagrant"
                puppet.hiera_config_path = "puppet/hiera.yaml"
                puppet.options = "--verbose --environment vagrant"
                puppet.working_directory = "/tmp/vagrant-puppet/environments"
                puppet.module_path = ["puppet/environments/vagrant/modules", "puppet/modules"]
            end
        end
    end
end
