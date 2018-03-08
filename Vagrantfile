Vagrant.configure("2") do |config|
    config.vm.box = "centos/7"
    config.vm.hostname = "vagrantbox"

    config.vm.network :forwarded_port, host: 80, guest: 80, auto_correct: true # website
    config.vm.network :forwarded_port, guest: 443, host: 443, auto_correct: true # ssl
    config.vm.network :forwarded_port, guest: 3306, host: 3306, auto_correct: true # mysql
    config.vm.network :private_network, ip: "10.0.0.10"
    config.vm.synced_folder "./", "/project", type: "rsync", id: "vagrant", :nfs => false,
        :mount_options => ["dmode=777", "fmode=666"]

    config.vm.provider "virtualbox" do |vb|
        vb.gui = false
        vb.customize ["modifyvm", :id, "--memory", "4096"]
        vb.customize ["modifyvm", :id, "--cpus", "4"]
        vb.customize ["modifyvm", :id, "--nictype1", "virtio"]
    end

    config.vm.provision "ansible" do |ansible|
        ansible.playbook = "ansible/playbook.yml"
        ansible.become = true
        ansible.extra_vars = {
          doc_root: "/project/public_html",
          mysql_root_password: "toor"
        }
    end

    config.vm.provision :shell, inline: "echo Good job, now enjoy your new vbox: http://10.0.0.10"

end
