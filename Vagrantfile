
Vagrant.configure("2") do |config|
  config.vm.network "private_network", :type => 'dhcp'
  config.vm.synced_folder ".", "/vagrant", disabled: false
  config.vm.box_check_update = false
  config.vbguest.auto_update = false
  config.vagrant.plugins = ["vagrant-hostmanager", "vagrant-vbguest"]

  config.vm.define "deployer-01" do |app01|
    app01.vm.box = "ubuntu/xenial64"
    app01.vm.hostname = "deployer-01"

    app01.vm.provider "virtualbox" do |v|
      v.name = "deployer-01"
      v.memory = 1024
      v.cpus = 1
    end
    # https://www.vagrantup.com/docs/provisioning/ansible_common.html
    app01.vm.provision "ansible" do |ansible|
      ansible.config_file = "playbooks/ansible.cfg"
      ansible.compatibility_mode = "2.0"
      ansible.host_key_checking = "false"
      ansible.verbose = ""
      ansible.playbook = "playbooks/iotms-playbook.yml"
    end
  end

  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.manage_guest = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true
  config.hostmanager.ip_resolver = proc do |vm, resolving_vm|
    `vagrant ssh #{vm.name} -c "hostname -I"`.split()[1]
  end
end
