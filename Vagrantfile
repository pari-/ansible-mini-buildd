require 'pathname'

HN = Pathname.new(Dir.pwd).basename.to_s.gsub('_','-');

VAGRANTFILE_API_VERSION = "2"

VM_COUNT = 1
VM_CPUS = 1
VM_MEM = 1024
VM_BASE_NAME = "vm"
VM_GROUP_NAME = "group"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  (1..VM_COUNT).each do |i|
    config.vm.define "#{VM_BASE_NAME}#{i}" do |subconfig|
      subconfig.vm.box = ENV['VAGRANT_CONFIG_VM_BOX'] || 'parallels/debian-9.3'
      subconfig.vm.hostname = (i == 1) ? HN : HN + i.to_s

      subconfig.vm.provider "parallels" do |prl|
        prl.cpus = VM_CPUS
        prl.memory = VM_MEM
      end

      subconfig.vm.provision :ansible do |ansible|
        if File.exists?("requirements.yml")
          ansible.galaxy_role_file = "requirements.yml"
        end
        ansible.groups = {
          "#{VM_GROUP_NAME}" => Array(1..VM_COUNT).map { |j| "#{VM_BASE_NAME}#{j}" }
        }
        ansible.become = true
        ansible.compatibility_mode = "2.0"
        ansible.playbook = "test.yml"
        ansible.tags = ENV['VAGRANT_ANSIBLE_TAGS'] || 'all'
      end
    end
  end
end
