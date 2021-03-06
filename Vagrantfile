# -*- mode: ruby -*-
# vi: set ft=ruby :

def file_dir_or_symlink_exists?(path_to_file)
  File.exist?(path_to_file) || File.symlink?(path_to_file)
end

Vagrant::Config.run do |config|
  if !file_dir_or_symlink_exists?("setuprc")
    print "A setuprc file for the install is missing.  Run './openstack_setup.sh' to generate one.\n\n"
    exit
  end
 
  # Setup virtual machine box in bridged mode. 
  config.vm.network :bridged
  config.vm.host_name = "chef-server" 
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  # Provision and install new kernel if deployment was not done
  if Dir.glob("#{File.dirname(__FILE__)}/.vagrant/machines/default/*/id").empty?
    config.vm.provision :shell, :path => "openstack_provision.sh"
  end
end

# Providers were added on Vagrant >= 1.1.0
Vagrant::VERSION >= "1.1.0" and Vagrant.configure("2") do |config|
  config.vm.provider :rackspace do |rs|
    config.ssh.private_key_path = ENV["RS_PRIVATE_KEY"]
    rs.username = ENV["RS_USERNAME"]
    rs.api_key  = ENV["RS_API_KEY"]
    rs.public_key_path = ENV["RS_PUBLIC_KEY"]
    rs.flavor   = /512MB/
    rs.image    = /Ubuntu/
  end

  config.vm.provider :virtualbox do |vb|
    config.vm.box = "precise64"
    config.vm.box_url = "http://files.vagrantup.com/precise64.box"
  end
end
