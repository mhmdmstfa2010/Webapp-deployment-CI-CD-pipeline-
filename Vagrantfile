Vagrant.configure("2") do |config|
  # Base box configuration
  base_box = "eurolinux-vagrant/centos-stream-9" # You can change this to another lightweight box

  ### VM 1 ###
  config.vm.define "vm1" do |vm1|
    vm1.vm.box = base_box
    vm1.vm.hostname = "vm1"
    vm1.vm.network "private_network", ip: "192.168.8.101"  # Static IP for VM 1
    vm1.vm.provider "virtualbox" do |vb|
      vb.memory = "256" # Minimal memory
      vb.cpus = 1       # Minimal CPU
    end
  end

  ### VM 2 ###
  config.vm.define "vm2" do |vm2|
    vm2.vm.box = base_box
    vm2.vm.hostname = "vm2"
    vm2.vm.network "private_network", ip: "192.168.8.102"  # Static IP for VM 2
    vm2.vm.provider "virtualbox" do |vb|
      vb.memory = "256" # Minimal memory
      vb.cpus = 1       # Minimal CPU
    end
  end
end

