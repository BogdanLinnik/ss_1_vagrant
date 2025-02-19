# -*- mode: ruby -*-
# vi: set ft=ruby :
# This is a standard metadata comment that ensures proper syntax highlighting in editors.

# Define a shell script to install dependencies (runs during provisioning)
$install_deps = <<-'SHELL'
  sudo apt-get update -y  # Updates the package lists to get the latest versions
  sudo apt-get install -y openjdk-17-jdk  # Installs OpenJDK 17 without asking for confirmation
SHELL

# Define a shell script to run the Gradle wrapper inside the /vagrant directory
$run_graldew = <<-'SHELL'
  cd /vagrant  # Navigate to the shared Vagrant directory
  ./gradlew bootRun  # Run the Gradle build tool with the `bootRun` task (for a Spring Boot app)
SHELL

# Begin Vagrant configuration
Vagrant.configure("2") do |config|
  # Specify the base image (Ubuntu 22.04 "Jammy Jellyfish")
  config.vm.box = "ubuntu/jammy64"

  # Set up a private network with a fixed IP address
  config.vm.network "private_network", ip: "192.168.100.100"

  # Configure the virtual machine resources
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 2048  # Allocate 2GB of RAM to the VM
    vb.cpus = 2       # Assign 2 CPU cores to the VM
  end

  # Provisioning: Run the installation script when the VM is set up
  config.vm.provision "shell", inline: $install_deps

  # Define a trigger to run after the VM has started
  config.trigger.after :up do |_trigger|
    # Run the Gradle wrapper to start the application
    config.vm.provision "shell", inline: $run_graldew
  end
end
