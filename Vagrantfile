Vagrant.configure("2") do |config|
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true

  ### app01 vm ###
  config.vm.define "app01" do |app01|
    app01.vm.box = "eurolinux-vagrant/centos-stream-9"
    app01.vm.box_version = "9.0.43"
    app01.vm.hostname = "app01"
    app01.vm.network "private_network", ip: "192.168.56.12"
    app01.vm.provider "virtualbox" do |vb|
      vb.memory = "800"
    end
  end

  ### app02 vm ###
  config.vm.define "app02" do |app02|
    app02.vm.box = "eurolinux-vagrant/centos-stream-9"
    app02.vm.box_version = "9.0.43"
    app02.vm.hostname = "app02"
    app02.vm.network "private_network", ip: "192.168.56.13"  # Changed IP
    app02.vm.provider "virtualbox" do |vb|
      vb.memory = "800"
    end
  end
end

