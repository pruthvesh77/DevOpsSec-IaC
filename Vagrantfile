Vagrant.configure("2") do |config|
  config.vm.boot_timeout = 600
  config.vm.box = "ubuntu/jammy64"
  config.vm.hostname = "devsecops-vm"

  config.vm.network "forwarded_port", guest: 5000, host: 5000
  config.vm.network "forwarded_port", guest: 9090, host: 9090

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = 2
  end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y python3 python3-pip docker.io ansible git unzip openjdk-21-jdk
    vb.cpus = 2 
  end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update --fix-missing
    apt-get install -y python3 python3-pip docker.io git unzip openjdk-21-jdk software-properties-common || apt-get install -y python3 python3-pip docker.io git unzip openjdk-21-jdk software-properties-common
    add-apt-repository --yes --update ppa:ansible/ansible
    apt-get update --fix-missing
    apt-get install -y ansible || apt-get install -y ansible
    groupadd -f docker
    usermod -aG docker vagrant
  SHELL
end
