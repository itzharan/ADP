	# Copyright 2016, EMC, Inc.

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
config.vm.define "default" do |target|
  config.vm.box = "centos/7"
  target.vm.box = "comiq/dockerbox"
  target.vm.box_version = "1.12.1.1"
	target.vm.provider "virtualbox" do |v|
            v.memory = 4096
            v.cpus = 4
            v.customize ["modifyvm", :id,
              "--nictype1", "virtio",
              "--nicpromisc2", "allow-all"
            ]
  end
  target.vm.network "private_network", ip: "172.31.128.1", virtualbox__intnet: "closednet"

        target.vm.network "forwarded_port", guest: 4243, host: 4243, id: "Docker Remote API"

        target.vm.network "forwarded_port", guest: 80, host: 80, id: "nginx"
        
        target.vm.synced_folder "..", "/Centos"
        target.vm.synced_folder '.', '/vagrant', disabled: true

        target.vm.provision :shell, inline: <<-SHELL
          #set -e # fail on error
          #set -x # debug commands
          #cp /lib/systemd/system/docker.service /docker.service.bak
          #sed -e \
           # 's/ExecStart=.*/ExecStart=\\/usr\\/bin\\/docker daemon -H fd:\\/\\/ -H tcp:\\/\\/0.0.0.0:4243/g' \
            #/lib/systemd/system/docker.service > /docker.service.sed
          #cp /docker.service.sed /lib/systemd/system/docker.service
          #systemctl daemon-reload
          #service docker restart
		  curl -fsSL https://get.docker.com/ | sh
		  sudo systemctl start docker
		  sudo systemctl status docker
		  #sudo docker build -t "simple_flask:dockerfile" -f H:\test\
		  sudo apt-get install nginx
        SHELL
end
end