# -*- mode: ruby -*-
# vi: set ft=ruby :

$install_deps = <<-'SHELL'
  sudo apt-get update -y
  sudo apt-get install -y openjdk-17-jdk
SHELL

$run_graldew = <<-'SHELL'
  cd /vagrant
  ./gradlew bootRun
SHELL

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"
  config.vm.network "private_network", ip: "192.168.100.100"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 2048
    vb.cpus = 2
  end

  config.vm.provision "shell", inline: $install_deps

  config.trigger.after :up do |_trigger|
    config.vm.provision "shell", inline: $run_graldew
  end
end
