# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

nodecfg = YAML::load_file('config.yml')
nodecfg['nodes'].each do |node|
    node.merge!(nodecfg['default']) { |key, nval, tval | nval }
    node['box_url'] = nodecfg['boxes'][node['box']]
    node['fqdn'] = node['name'] + '.' + node['domain']
end

Vagrant.configure("2") do |config|

    nodecfg['nodes'].each do |node|
        config.vm.define node['name'] do |node_config|
            node_config.vm.hostname = node['name']
            node_config.vm.box = node['box']
            node_config.vm.box_url = node['box_url']
            node_config.vm.network :private_network, ip: node['ip']
            node_config.vm.provision :hosts

            node_config.vm.provider :virtualbox do |vb|
                if  node['cpus'].to_i > 1
                    vb.customize ["modifyvm", :id, "--ioapic", "on"]
                end
                vb.customize ["modifyvm", :id, "--cpus", node['cpus']]
                vb.customize ["modifyvm", :id, "--memory", node['memory']]
                vb.customize ["modifyvm", :id, "--name", node['name']]
            end


# uncomment to use puppet
# =======================
#            node_config.vm.provision :puppet do |puppet|
#                puppet.manifests_path = "provision/puppet/manifests"
#                puppet.module_path = "provision/puppet/modules"
#                puppet.manifest_file  = "init.pp"
#            end


# uncomment to use chef
# =====================
#            node_config.vm.provision :chef_solo do |chef|
#                chef.cookbooks_path = "provision/chef/cookbooks"
#            end


        end
    end
end
