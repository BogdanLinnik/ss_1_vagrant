# -*- mode: ruby -*-
# vi: set ft=ruby :
# Vagrant configuration file to set up a development environment.

# Shell script to install dependencies (runs during provisioning)
$install_deps = <<-'SHELL'
  set -e  # Exit immediately if a command exits with a non-zero status (fail-fast)
  
  # Update package lists and install dependencies
  echo "Updating package lists..."
  sudo apt-get update -y
  
  echo "Installing OpenJDK 17..."
  sudo DEBIAN_FRONTEND=noninteractive apt-get install -y openjdk-17-jdk
  
  echo "Dependency installation completed successfully!"
SHELL

# Shell script to run the Gradle wrapper inside the shared /vagrant directory
$run_gradlew = <<-'SHELL'
  set -e  # Fail on first error
  
  cd /vagrant  # Navigate to the shared project directory
  
  if [ ! -f "./gradlew" ]; then
    echo "Error: Gradle wrapper (gradlew) not found!"
    exit 1
  fi
  
  echo "Starting the application with Gradle..."
  ./gradlew bootRun
SHELL

# Begin Vagrant configuration
Vagrant.configure("2") do |config|
  # Use Ubuntu 22.04 (Jammy Jellyfish) as the base image
  config.vm.box = "ubuntu/jammy64"

  # Set up a private network with a fixed IP address
  config.vm.network "private_network", ip: "192.168.100.100"

  # Configure VM resources
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 2048  # Allocate 2GB of RAM
    vb.cpus = 2       # Assign 2 CPU cores
  end

  # Enable SSH access via the private network
  config.ssh.host = "192.168.100.100"
  config.ssh.insert_key = false

  # Provisioning: Install dependencies when the VM is created
  config.vm.provision "shell", inline: $install_deps

  # Ensure the Gradle application starts only after the VM is fully up
  config.trigger.after :up do |_trigger|
    config.vm.provision "shell", inline: $run_gradlew
  end
end
