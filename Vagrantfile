Vagrant.configure(2) do |config|
  config.vm.box     = "centos72"
  config.vm.box_url = "https://github.com/CommanderK5/packer-centos-template/releases/download/0.7.2/vagrant-centos-7.2.box"

  config.vm.network :private_network, ip: "192.168.33.10"

  config.vm.synced_folder "./playbook", "/vagrant/ansible", :mount_options => ["dmode=775","fmode=664"]

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "4096"
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
