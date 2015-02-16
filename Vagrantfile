# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.define "web" do |web|
    web.vm.network "private_network", ip: "192.168.33.2"
    web.vm.network "forwarded_port", guest: 8000, host: 8000
    web.vm.synced_folder "../mongo-express-app", "/app"
    web.vm.provision "shell", inline: <<-SCRIPT
      sudo apt-get install -y nodejs
      cd /app; npm start & 
SCRIPT
  end

  config.vm.define "db" do |db|
    db.vm.network "private_network", ip: "192.168.33.1"
    db.vm.provision "shell", inline: <<-SCRIPT
      sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
      echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' | sudo tee /etc/apt/sources.list.d/mongodb.list
      sudo apt-get update
      sudo apt-get install -y mongodb-org
      sudo sed -i "s/bind_ip = 127.0.0.1/bind_ip=0.0.0.0/g" /etc/mongod.conf
      sudo service mongod stop && sudo service mongod start
SCRIPT
  end
end
