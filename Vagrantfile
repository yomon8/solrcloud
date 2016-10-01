# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "centos/7"

  config.vm.define "zk1" do | s |
    s.vm.network 'private_network', ip: "192.168.110.100"
    s.vm.hostname = "zk1"
    s.vm.provider 'virtualbox' do |vb|
      vb.memory = '512'
    end
    s.vm.provision "ansible" do |ansible|
      ansible.verbose = 'v'
      ansible.playbook = "ansible/site.yml"
      ansible.inventory_path = "ansible/hosts"
      ansible.limit = 'zookeepers'
      ansible.raw_ssh_args = ['-F sshconfig']
    end
  end

  solr_num = 2
  (1..solr_num).each do |i|
    config.vm.define "solr#{i}" do | s |
      s.vm.network 'private_network', ip: "192.168.110.1#{i}"
      s.vm.hostname = "solr#{i}"
      s.vm.provider 'virtualbox' do |vb|
      vb.memory = '1024'
      end
      if i == solr_num
        s.vm.provision "ansible" do |ansible|
          ansible.verbose = 'v'
          ansible.playbook = "ansible/site.yml"
          ansible.inventory_path = "ansible/hosts"
          ansible.limit = 'solr_servers'
          ansible.raw_ssh_args = ['-F sshconfig']
        end
      end
    end
  end
end

