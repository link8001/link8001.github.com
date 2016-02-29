# -*- mode: ruby -*-
# vi: set ft=ruby :
#
Vagrant.configure(2) do |config|
  config.vm.box = "trusty64"
  config.vm.hostname = "blog"
  config.vm.provider "virtualbox" do |vb|
    vb.linked_clone = true
    vb.name = "blog"
  end
  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get install -y git
    git config --global user.email "lyallchan@163.com"
    git config --global user.name "lyallchan@blog"
    git clone https://github.com/creationix/nvm.git /home/vagrant/.nvm
    echo >> /home/vagrant/.bashrc
    echo 'export NVM_DIR="$HOME/.nvm"' >> /home/vagrant/.bashrc
    echo '[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"' >> /home/vagrant/.bashrc
    . /home/vagrant/.nvm/nvm.sh
    nvm install 4.2.4
    nvm use 4.2.4
    nvm alias default 4.2.4
    npm install cnpm -g --registry=https://registry.npm.taobao.org
    cnpm install hexo-cli -g 
    cd /vagrant
    cnpm install
  SHELL
end
