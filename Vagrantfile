# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "generic/oracle8"
  config.vm.network "forwarded_port", guest: 8888, host: 8888, host_ip: "0.0.0.0"
  config.vm.synced_folder ".", "/vagrant", type: "rsync"

  config.vm.provider "virtualbox" do |vb|
    # Customize the amount of memory on the VM:
    vb.memory = "8192"
  end
  config.vm.provision "shell", inline: <<-SHELL

  	useradd -U jupyter
	export PATH="/usr/local/bin:$PATH"
	dnf -y install libffi-devel sqlite-devel openssl-devel bzip2-devel zlib-devel gcc make
	wget -q https://www.python.org/ftp/python/3.9.16/Python-3.9.16.tgz
	tar xzf Python-3.9.16.tgz
	cd Python-3.9.16
	./configure --enable-optimizations
	make altinstall

	python3.9 -m pip install --upgrade pip
	pip3.9 install -r /vagrant/requirements.txt
	pip3.9 install -r /vagrant/service/requirements-jupyter.txt

	mkdir /notebooks
	cp /vagrant/necs_data.csv /vagrant/necs_tutorial_analysis.ipynb /notebooks
	chown -R jupyter /vagrant /notebooks

	cp /vagrant/service/jupyter-notebook.service /usr/lib/systemd/system
	systemctl enable --now jupyter-notebook.service
        firewall-cmd --permanent --add-port 8888/tcp
        firewall-cmd --reload

  SHELL
end
