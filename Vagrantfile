
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
    node.vm.hostname = 'laravel'
    # Assign Static Address
    node.vm.network "private_network", ip: "192.168.33.10"
    #node.vm.network "private_network", ip: "dhcp"	
    # Create address for this host
    node.hostmanager.aliases = %w(laravel.lan)

  end

  # Specify the base box
  config.vm.box = "ubuntu/trusty64"

  # Setup port forwarding
  config.vm.network "forwarded_port", guest: 80, host: 8080, auto_correct: true

  # Setup network
  #private network provissioned above for hostmanager purpose
  #config.vm.network "private_network", ip: "192.168.33.10"

  # Setup synced folder
    config.vm.synced_folder "www/", "/var/www/html", group: "www-data", owner: "vagrant", :mount_options => ['dmode=775', 'fmode=775']
    config.vm.synced_folder "config/", "/var/www/config", group: "www-data", owner: "vagrant", :mount_options => ['dmode=775', 'fmode=775']



  # CUSTOMIZATION
   config.vm.provider "virtualbox" do |vb|

  # Allowing internet access
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]

     vb.name = "devspace"
  
     # Customize the amount of memory on the VM:
     vb.memory = "2048"
     vb.cpus = 1
   end


  # PROVISION
  # config.vm.provision :shell, path: “vagrant/bootstrap.sh"
   # Shell provisioning
    config.vm.provision "shell" do |s|
      s.path = "bootstrap.sh"
    end
  
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   sudo apt-get update
  #   sudo apt-get install -y apache2
  # SHELL
end
