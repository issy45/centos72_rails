Vagrant.configure(2) do |config|
  config.vm.box = "geerlingguy/centos7"

  config.vm.network :private_network, ip: "192.168.33.10"

  config.vm.synced_folder "./ansible", "/vagrant/ansible", :mount_options => ["dmode=775","fmode=664"]

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
  end

  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook        = "/vagrant/ansible/site.yml"
    ansible.verbose         = true
    ansible.install         = true
    ansible.limit           = "all" # or only "nodes" group, etc.
    ansible.inventory_path  = "/vagrant/ansible/hosts"
  end
end
