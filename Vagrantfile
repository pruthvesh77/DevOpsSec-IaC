Vagrant.configure("2") do |config|
  config.vm.define "webserver" do |webserver|
    webserver.vm.box = "ubuntu/noble64" # Ubuntu 24.04 LTS
    webserver.vm.hostname = "webserver"
    webserver.vm.network "private_network", ip: "192.168.56.10"
    webserver.vm.provider "virtualbox" do |vb|
      vb.name = "DevOpsSec-Webserver"
      vb.memory = "2048"
      vb.cpus = "2"
    end
  end

  config.vm.define "dbserver" do |dbserver|
    dbserver.vm.box = "ubuntu/noble64" # Ubuntu 24.04 LTS
    dbserver.vm.hostname = "dbserver"
    dbserver.vm.network "private_network", ip: "192.168.56.20"
    dbserver.vm.provider "virtualbox" do |vb|
      vb.name = "DevOpsSec-DBserver"
      vb.memory = "2048"
      vb.cpus = "2"
    end
  end

  # Ansible Provisioner for Infrastructure as Code
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "ansible/playbook.yml"
    ansible.inventory_path = "ansible/inventory.ini"
    ansible.limit = "all"

    # Point to the Ansible executable within the WSL virtual environment
    # This is CRITICAL for WSL + venv setup
    # NOTE: /home/mossad/.venv/bin/ansible-playbook assumes your WSL username is 'mossad'
    # Adjust 'mossad' to your actual WSL username!
    ansible.ansible_keep_remote_latest = true # Keep latest Ansible on guest for local provisioner
    ansible.provision_ansible = "install" # Install Ansible on the guest if not present

    # Provide the path to the python interpreter on the guest VM
    ansible.python_interpreter = "/usr/bin/python3"

    # Pass the vault password file. Vagrant automatically handles pathing from host to guest.
    ansible.raw_args = ["--vault-password-file", "ansible/.vault_pass"]
  end
end