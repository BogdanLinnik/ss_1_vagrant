# -*- mode: ruby -*-
# vi: set ft=ruby :

# Скрипт установки Java 17 и необходимых зависимостей

$install_deps = <<-'SHELL'
 sudo apt-get update -y
 sudo apt-get install -y openjdk-17-jdk
SHELL

Vagrant.configure("2") do |config|
  # Использование базового образа Ubuntu 22.04 (Jammy Jellyfish)
  config.vm.box = "ubuntu/jammy64"

  # Настройка приватной сети с фиксированным IP-адресом
  config.vm.network "private_network", ip: "192.168.100.100"

  # Проброс порта 8080 с гостевой ОС на хост
  config.vm.network "forwarded_port", guest: 80, host: 8080

  # Конфигурация VirtualBox
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 2048  # Выделение 4 ГБ оперативной памяти
    vb.cpus = 2       # Использование 2 ядер процессора
  end

  # Установка зависимостей при первом запуске ВМ
  config.vm.provision "shell", inline: $install_deps

  # Создание systemd-сервиса для запуска приложения
  config.vm.provision "shell", inline: <<-'SHELL'
  sudo bash -c 'cat <<EOF > /etc/systemd/system/gradle-app.service
[Unit]
Description=Gradle Spring Boot Application
After=network.target

[Service]
User=vagrant
WorkingDirectory=/vagrant
ExecStart=/vagrant/gradlew bootRun
Restart=always
StandardOutput=journal
StandardError=journal
Environment=JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
Environment=PATH=/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/usr/lib/jvm/java-17-openjdk-amd64/bin

[Install]
WantedBy=multi-user.target
EOF'

   # Reload systemd, enable and start the service
   sudo systemctl daemon-reload
   sudo systemctl enable gradle-app.service
   sudo systemctl start gradle-app.service

   echo "Systemd service for Gradle application created and started successfully!"
 SHELL
end
