# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
  :master => {
        :box_name => "bento/ubuntu-22.04",
        :ip_addr => '192.168.56.150'
  },
  :slave => {
        :box_name => "bento/ubuntu-22.04",
        :ip_addr => '192.168.56.151'
  }
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

      config.vm.define boxname do |box|

          box.vm.box = boxconfig[:box_name]
          box.vm.host_name = boxname.to_s

          #box.vm.network "forwarded_port", guest: 3260, host: 3260+offset

          box.vm.network "private_network", ip: boxconfig[:ip_addr]

          box.vm.provider :virtualbox do |vb|
            vb.memory = "1024"
          end
          
          ssh_pub_key = File.readlines("../id_rsa.pub").first.strip
          box.vm.provision "shell", inline: <<-SHELL
            mkdir ~root/.ssh
            echo #{ssh_pub_key} >> ~vagrant/.ssh/authorized_keys
            echo #{ssh_pub_key} >> ~root/.ssh/authorized_keys
            sudo sed -i 's/\#PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
            systemctl restart sshd
          SHELL

      end
  end
end