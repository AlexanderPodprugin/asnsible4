# -*- mode: ruby -*-
# vim: set ft=ruby :
MACHINES = {
  :consul1 => {
        :box_name => "redos732",
        :ip_addr => '192.168.11.191'
  },
  :consul2 => {
        :box_name => "redos732",
        :ip_addr => '192.168.11.192'
  },
  :consul3 => {
        :box_name => "redos732",
        :ip_addr => '192.168.11.193'
  },
  :pg1 => {
        :box_name => "redos732",
        :ip_addr => '192.168.11.181'
  },
  :pg2 => {
        :box_name => "redos732",
        :ip_addr => '192.168.11.182'
  },
  :nfs => {
        :box_name => "redos732",
        :ip_addr => '192.168.11.188'
  }
}

Vagrant.configure("2") do |config|
  MACHINES.each do |boxname, boxconfig|
    config.vm.define boxname do |box|
        box.vm.box = boxconfig[:box_name]
        box.vm.host_name = boxname.to_s
        box.vm.network "private_network", ip: boxconfig[:ip_addr]
        box.vm.provider "virtualbox" do |v|
          v.memory = 256
        end
        case boxname.to_s
        when "pg1"
            box.vm.disk :disk, size: "10GB", name: "pg1_data"
            box.vm.disk :disk, size: "1GB", name: "pg1_wal"
            box.vm.provider "virtualbox" do |v|
                  v.memory = 512
            end
        when "pg2"
            box.vm.disk :disk, size: "10GB", name: "pg2_data"
            box.vm.disk :disk, size: "1GB", name: "pg2_wal"
            box.vm.provider "virtualbox" do |v|
                  v.memory = 512
            end
        when "nfs"
            box.vm.disk :disk, size: "10GB", name: "nfs_backup"
            box.vm.provider "virtualbox" do |v|
                  v.memory = 512
            end
        end
    end
    config.vm.provision "shell" do |s|
       ssh_pub_key = File.readlines("/home/mario/.ssh/id_rsa.pub").first.strip
       s.inline = <<-SHELL
         mkdir -p /home/vagrant/.ssh
         sudo mkdir -p /root/.ssh
         echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
         echo #{ssh_pub_key} >> /root/.ssh/authorized_keys
       SHELL
    end
  end
end
