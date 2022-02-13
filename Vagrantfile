$script = <<-SCRIPT
cd ~/homebox
for f in defaults/*.default; do base=${f##*/} base=${base%.default}; cp -n -- "$f" "$base"; done
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.synced_folder ".", "/vagrant", disabled: false
  config.vm.synced_folder ".", "/home/vagrant/homebox", disabled: false
  config.vm.provision "shell", path: "https://homebox.works/scripts/dep.sh"
  config.vm.provision "shell", privileged: false, inline: $script
  config.vm.provision "ansible_local", run: "always" do |ansible|
    ansible.compatibility_mode = "2.0"
    ansible.config_file = "/home/vagrant/homebox/ansible.cfg"
    ansible.inventory_path = "/home/vagrant/homebox/inventories/local"
    ansible.limit = "all"
    ansible.playbook = "/home/vagrant/homebox/homebox.yml"
    ansible.become = true
    # ansible.verbose Examples: false, true (equivalent to v), -vvv (equivalent to vvv), vvvv.
    ansible.verbose = false
    ansible.tags = "homebox"
    ansible.skip_tags = "settings"
    ansible.extra_vars = { continuous_integration: true }

  end
#  config.vm.define "ubuntu18" do |ubuntu18|
#    ubuntu18.vm.box = "generic/ubuntu1804"
#  end
   config.vm.define "ubuntu16" do |ubuntu16|
     ubuntu16.vm.box = "generic/ubuntu1604"
   end
end
