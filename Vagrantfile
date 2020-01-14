# -*- mode: ruby -*-
# vi: set ft=ruby :


def gui_enabled?
  !ENV.fetch('GUI', '').empty?
end

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.provider "virtualbox" do |vb|
    vb.gui = gui_enabled?
    vb.memory = "2048"
    vb.cpus = 8
    config.disksize.size = '40GB'
  end

  config.vm.provision "shell", env: {"UCF_FORCE_CONFFNEW" => "true"}, inline: <<-SHELL
    apt update
    apt build-dep -y linux linux-image-$(uname -r)
    apt install -y libncurses-dev flex bison openssl libssl-dev dkms libelf-dev
    apt install -y libudev-dev libpci-dev libiberty-dev autoconf debconf-utils
    # Important install debconf-utils before kernel-package to remove ncurses interface
    echo "Before debconf-set-selections"
    echo "ucf     ucf/changeprompt        select  keep_current" | debconf-set-selections
    apt install -y kernel-package fakeroot
    apt remove --purge grub-legacy-ec2
    git clone git://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git ~vagrant/linux-stable
    chown -R vagrant: ~vagrant/linux-stable
  SHELL
  config.vm.synced_folder "../learning_module", "/home/vagrant/learning_module"
end
