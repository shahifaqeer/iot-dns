# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

Vagrant.configure(2) do |config|
  
  # 64 bit Vagrant Box
  config.vm.box = "ubuntu/trusty64"

  ## Guest Config
  config.vm.hostname = "vighata"
  config.vm.network "forwarded_port", guest: 8888, host: 9888
  config.ssh.forward_x11 = true
 
  ## Provisioning
  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get install -y python-dev
    sudo apt-get install -y python-pip
    sudo pip install --upgrade ipython[all]
    sudo apt-get install -y apache2-utils
    # The commands below are for assignment 2. We expect that the
    # lines above were already executed successfully during assignment 1.
    # If you are starting a new vm, uncomment the lines above that
    # start with sudo.
     sudo apt-get install -y software-properties-common
     sudo add-apt-repository ppa:webupd8team/java
     sudo apt-get update
     sudo apt-get install -y python-dateutil
     sudo pip install test_helper
     sudo apt-get install -y python-matplotlib
     echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | sudo /usr/bin/debconf-set-selections
     sudo apt-get install -y -q oracle-java8-installer
     wget http://d3kbcqa49mib13.cloudfront.net/spark-1.6.1-bin-hadoop2.6.tgz
     tar xvf spark-1.6.1-bin-hadoop2.6.tgz
     mv spark-1.6.1-bin-hadoop2.6 spark
     rm spark-1.6.1-bin-hadoop2.6.tgz
  SHELL
  
  ## iSDX
  
  # mount platform directory
  config.vm.synced_folder ".", "/home/vagrant/dev"
  
  ## Notebook
  config.vm.provision "shell", run: "always", inline: <<-SHELL
    #ipython notebook --notebook-dir=/vagrant/notebook --no-browser --ip=0.0.0.0 &
    cd /vagrant/notebook; sudo JAVA_HOME=/usr/lib/jvm/java-8-oracle IPYTHON_OPTS="notebook --ip=0.0.0.0 --no-browser" /home/vagrant/spark/bin/pyspark &
  SHELL
  
  ## CPU & RAM
  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--cpuexecutioncap", "100"]
    vb.memory = 4096
    vb.cpus = 1
  end
end
