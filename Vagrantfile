Vagrant.configure("2") do |config|
  # Define the webserver VM
  config.vm.define "webserver" do |webserver|
    webserver.vm.box = "ubuntu/jammy64"
    webserver.vm.hostname = "webserver"
    webserver.vm.network "private_network", ip: "192.168.56.10"
    webserver.vm.provider "virtualbox" do |vb|
      vb.name = "Mossad-Webserver"
      vb.memory = "3072" # Increased memory for Ansible control node
      vb.cpus = "2"
      vb.gui = true
    end

    # Shell provisioner to install Ansible and set up vault on webserver
    webserver.vm.provision "shell", inline: <<-SHELL
      echo "Starting Ansible installation on webserver..."
      sudo apt-get update -y
      sudo apt-get install -y software-properties-common python3 python3-pip
      sudo add-apt-repository --yes --update ppa:ansible/ansible
      sudo apt-get install -y ansible

      # Create and set up vault password file for Ansible inside VM
      # IMPORTANT: Replace "your_secure_vault_password" with your actual vault password
      echo "your_secure_vault_password" > /tmp/vagrant_vault_pass.txt
      chmod 600 /tmp/vagrant_vault_pass.txt
      # Set the environment variable permanently for the vagrant user
      echo 'export ANSIBLE_VAULT_PASSWORD_FILE=/tmp/vagrant_vault_pass.txt' >> /home/vagrant/.bashrc
      # Source it immediately for the current shell (if executed from here, after a fresh login it loads from .bashrc)
      source /home/vagrant/.bashrc
      echo "Ansible installation and vault setup complete on webserver."
    SHELL
  end

  # Define the dbserver VM
  config.vm.define "dbserver" do |dbserver|
    dbserver.vm.box = "ubuntu/jammy64"
    dbserver.vm.hostname = "Mossad-dbserver"
    dbserver.vm.network "private_network", ip: "192.168.56.20"
    dbserver.vm.provider "virtualbox" do |vb|
      vb.name = "DevOpsSec-DBserver"
      vb.memory = "2048"
      vb.cpus = "2"
      vb.gui = true
    end
    # No Ansible provisioner here; dbserver will be provisioned by webserver
  end
end