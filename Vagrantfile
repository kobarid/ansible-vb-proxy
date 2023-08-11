Vagrant.configure("2") do |config|
  (1..3).each do |index|
    config.vm.define "vm#{index}" do |vm|
      vm.vm.box = "ubuntu/focal64"
      vm.vm.network "private_network", ip: "192.168.56.#{100 + index}"

      vm.vm.provision "shell" do |s|
        ssh_pub_key = File.readlines("/home/ubuntu/.ssh/id_rsa.pub").first.strip
        s.inline = <<-SHELL
          echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
          echo #{ssh_pub_key} >> /root/.ssh/authorized_keys
        SHELL
      end
    end
  end
end
