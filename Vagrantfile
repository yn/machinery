# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  config.vm.provision "vm", type: :ansible do |ansible|
    ansible.playbook = "vm.yml"
    ansible.extra_vars = {
      ansible_connection: 'ssh',
      ansible_ssh_args: '-o ForwardAgent=yes'
    }
  end
  config.vm.define "default", primary: true do |b|
    b.vm.provider :aws do |aws|
      aws.tags = {Name: "#{ENV['USER']}'s current box"}
      #aws.instance_type="g2.2xlarge"
    end
  end
  config.vm.define "nextgen" do |b|
    b.vm.provider :aws do |aws|
      aws.tags = {Name: "#{ENV['USER']}'s nextgen box"}
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
      region.ami = 'ami-27150617'
      region.keypair_name = ENV['USER']
    end
    aws.instance_type="t1.micro"
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

  config.vm.provider :digital_ocean do |provider, override|
    override.vm.box = 'digital_ocean'
    override.vm.box_url = "https://github.com/smdahlen/vagrant-digitalocean/raw/master/box/digital_ocean.box"
    #override.vm.hostname = "mosaic"
    #override.vm.host_name = "mosaic"
    #override.ssh.username = "mosaic"
    override.ssh.private_key_path = ["~/.ssh/id_rsa", "~/.ssh/id_dsa"].map do |s|
      File.expand_path(s)
    end.find do |s|
      !File.directory?(s) && File.readable?(s)
    end

    provider.client_id = ENV["DIGITAL_OCEAN_CLIENT_ID"]
    provider.api_key = ENV["DIGITAL_OCEAN_API_KEY"]
    provider.image = "Ubuntu 14.04 x64"
  end
end
