# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  # config.vm.box = "base"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   sudo apt-get update
  #   sudo apt-get install -y apache2
  # SHELL
  (1..4).each do |box_id|
    box_name = "devbox#{box_id}"
    config.vm.define box_name, primary: (1 == box_id) do |b|
      b.vm.provider :aws do |aws|
        aws.tags = {Name: "#{ENV['USER']}'s #{box_name}"}
        case box_id
        when 1..4
          aws.region_config "us-west-2" do |region|
            region.ami = 'ami-27150617'
          end
          aws.instance_type="g2.2xlarge"
          aws.ebs_optimized = true
          aws.block_device_mapping = [ 
            {
              'DeviceName' => "/dev/sda1",
              'VirtualName' => "root",
              'Ebs.VolumeType' => 'gp2',
              'Ebs.VolumeSize' => 60,
              'Ebs.DeleteOnTermination' => true
            }
          ]      
        end
      end
    end
  end
  
  config.vm.provider :aws do |aws, override|
    override.nfs.functional = false
    override.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"
    override.vm.box = "dummy"
    override.ssh.username = "ubuntu"
    override.ssh.private_key_path = "~/.ssh/id_rsa"
    
    aws.access_key_id = ENV["PERSONAL_ACCESS_KEY"]
    aws.secret_access_key = ENV["PERSONAL_SECRET_KEY"]
    aws.region = "us-west-2"
    aws.region_config "us-west-2" do |region|
      region.keypair_name = ENV['USER']
    end
  end
end
