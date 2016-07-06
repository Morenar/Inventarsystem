# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'json'
require 'yaml'

VAGRANTFILE_API_VERSION = "2"
confDir = $confDir ||= File.expand_path(File.dirname(__FILE__) + '/vagrant/vagrant_config')

homesteadYamlPath = confDir + "/Homestead.yaml"
homesteadJsonPath = confDir + "/Homestead.json"
afterScriptPath = confDir + "/after.sh"
aliasesPath = confDir + "/aliases"

require File.expand_path(File.dirname(__FILE__) + '/vagrant/scripts/homestead.rb')

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    if File.exists? aliasesPath then
        config.vm.provision "file", source: aliasesPath, destination: "~/.bash_aliases"
    end

    if File.exists? homesteadYamlPath then
        settings = YAML::load(File.read(homesteadYamlPath))
    elsif File.exists? homesteadJsonPath then
        settings = JSON.parse(File.read(homesteadJsonPath))
    end

    Homestead.configure(config, settings)

    if File.exists? afterScriptPath then
        config.vm.provision "shell", path: afterScriptPath, privileged: false
    end

    if defined? VagrantPlugins::HostsUpdater
        config.hostsupdater.aliases = settings['sites'].map { |site| site['map'] }
    end
end

Vagrant.configure("2") do |config|
  # ... other configuration
  # this runs shell commands
  config.vm.provision "shell", inline: "echo vagrant server is installed"
  config.vm.provision "shell", inline: "echo downloading git repository from URL"
  config.vm.provision "shell", inline: "cd code"
  # config.vm.provision "shell", inline: "git clone https://github.com/Morenar/Inventarsystem.git --branch laravel --single-branch code/laravel"
  config.vm.provision "shell", inline: "echo newest git repository from https://github.com/Morenar/Inventarsystem.git --branch laravel was downloaded"
  config.vm.provision "shell", inline: "echo ready to code"
  
end
