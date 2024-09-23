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

    # Forward ports for Grafana and Prometheus
    soufe1.vm.network "forwarded_port", guest: 8080, host: 8080  # HTTP for Grafana and Prometheus
    soufe1.vm.network "forwarded_port", guest: 8443, host: 8443   # HTTPS for Grafana and Prometheus

    # Forward ports for HAProxy Stats
    soufe1.vm.network "forwarded_port", guest: 8404, host: 8404  # HTTP Stats for HAProxy
    soufe1.vm.network "forwarded_port", guest: 9443, host: 9443  # HTTPS Stats for HAProxy

    soufe1.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
      vb.cpus = 1
    end 
  end

  # Provisioning with Ansible
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.extra_vars = {
      ansible_ssh_common_args: '-o StrictHostKeyChecking=no',
      ansible_python_interpreter: '/usr/bin/python3',
      haproxy_frontend_http_port: 8080,   # HTTP port for Grafana and Prometheus
      haproxy_frontend_https_port: 443,   # HTTPS port for Grafana and Prometheus
      haproxy_stats_port: 8404            # HTTP Stats port for HAProxy
    }
    ansible.limit = "all"
    ansible.become = true
    ansible.compatibility_mode = "2.0"
  end
end
