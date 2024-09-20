Vagrant.configure("2") do |config|
  config.vm.define "soube2" do |soube2|
    soube2.vm.box = "rockylinux/9"
    soube2.vm.hostname = "soube2"
    soube2.vm.network "private_network", ip: "192.168.57.21"
    
    soube2.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
      vb.cpus = 1
    end
  end
  
  config.vm.define "soufe1" do |soufe1|
    soufe1.vm.box = "rockylinux/9"
    soufe1.vm.hostname = "soufe1"
    soufe1.vm.network "private_network", ip: "192.168.57.22"

    soufe1.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
      vb.cpus = 1
    end 
  end

  # Provisioning with Ansible
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"  # The Ansible playbook to run
    ansible.extra_vars = {
      ansible_ssh_common_args: '-o StrictHostKeyChecking=no',
      ansible_python_interpreter: '/usr/bin/python3'
    }
    ansible.limit = "all"
    ansible.become = true  # Ensure that Ansible uses become instead of sudo
    ansible.compatibility_mode = "2.0"  # Use Ansible compatibility mode 2.0
  end
end
