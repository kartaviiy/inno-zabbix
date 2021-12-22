IMAGE_NAME = "bento/ubuntu-20.04"
N = 1

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    config.vm.provider "virtualbox" do |v|
        v.memory = 4096
        v.cpus = 4
    end

    config.vm.define "bastion" do |bastion|
        bastion.vm.box = IMAGE_NAME
        bastion.vm.network "private_network", ip: "192.168.56.3"
        bastion.vm.hostname = "bastion"
        bastion.vm.synced_folder ".", "/vagrant", create: true, type: "virtualbox"
        bastion.vm.provision "ansible" do |ansible|
            ansible.playbook = "bastion.yml"
            ansible.extra_vars = {
                node_ip: "192.168.56.3",
                ansible_python_interpreter: "/usr/bin/python3",
            }
        end
    end

    (1..N).each do |i|
        config.vm.define "node-#{i}" do |node|
            node.vm.box = IMAGE_NAME
            node.vm.network "private_network", ip: "192.168.56.85"
            node.vm.hostname = "node-#{i}"
            node.vm.provision "ansible" do |ansible|
                ansible.verbose = "vv"
                ansible.playbook = "node.yml"
                ansible.extra_vars = {
                    node_ip: "192.168.56.85",
                    ansible_python_interpreter: "/usr/bin/python3",
                }
            end
        end
    end
end