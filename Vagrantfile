#Load settings from yaml config file
require 'yaml'

settings = YAML.load_file 'config.yml'
vm_host_name = settings['vm']['host_name']
vm_ip_address = settings['vm']['ip_address']
vm_name = settings['vm']['name']
vm_memory = settings['vm']['memory']
vm_cpu_cores = settings['vm']['cpu_cores']

Vagrant.configure(2) do |config|
  # Configuration for vagrant plugin hostmanager
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.manage_guest = false
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = false
  
  # Actual hostname and address configuration
  config.vm.define 'Laravel' do |node|
    # Create Hostname
    node.vm.hostname = vm_host_name
    # Assign Static Address
    node.vm.network "private_network", ip: vm_ip_address
  end

  # Specify the base box
  config.vm.box = "ubuntu/trusty64"

  # Setup port forwarding
  config.vm.network "forwarded_port", guest: 80, host: 8080, auto_correct: true

  # Setup synced folder
    config.vm.synced_folder "www/", "/var/www/html", group: "www-data", owner: "vagrant", :mount_options => ['dmode=775', 'fmode=775']
    config.vm.synced_folder "config/", "/var/www/config", group: "www-data", owner: "vagrant", :mount_options => ['dmode=775', 'fmode=775']


  # CUSTOMIZATION
   config.vm.provider "virtualbox" do |vb|

  # Allowing internet access
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]

     vb.name = vm_name
  
     # Customize the amount of memory on the VM:
     vb.memory = vm_memory
     vb.cpus = vm_cpu_cores
   end


  # PROVISION
  # config.vm.provision :shell, path: “vagrant/bootstrap.sh"
   # Shell provisioning
    config.vm.provision "shell" do |s|
      s.path = "bootstrap.sh"
    end  
end
