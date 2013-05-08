# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

nodecfg = YAML::load_file('config.yml')
nodecfg['nodes'].each do |node|
    node.merge!(nodecfg['template']) { |key, nval, tval | nval }
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
        end
    end
end
