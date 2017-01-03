Vagrant.configure(2) do |config|

  config.vm.provider :virtualbox do |vbox|
    vbox.gui = false
    vbox.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vbox.name = "activemq-server"
  end

  config.vm.define "activemq" do |activemq|
    activemq.vm.box = "ubuntu/trusty32"
    activemq.vm.network "private_network", ip: "10.0.10.34"
    activemq.vm.network :forwarded_port, guest: 8161, host: 8161, auto_correct: true
    activemq.vm.network :forwarded_port, guest: 6166, host: 6166, auto_correct: true
    activemq.vm.network :forwarded_port, guest: 6163, host: 6163, auto_correct: true
  end

config.vm.provision :shell, inline: <<-SHELL
    cd /home/vagrant/ && wget https://archive.apache.org/dist/activemq/5.14.1/apache-activemq-5.14.1-bin.tar.gz
    cd /home/vagrant/ && tar -zxvf apache-activemq-5.14.1-bin.tar.gz
    cd /home/vagrant/apache-activemq-5.14.1/bin && chmod 755 activemq
    sudo apt-get update
    sudo apt-get install default-jdk -y
    echo 'JAVA_HOME="/usr/lib/jvm/java-7-openjdk-i386/"' | sudo tee -a /etc/environment
    source /etc/environment
    cd /home/vagrant/apache-activemq-5.14.1/bin && sudo sh activemq start
  SHELL

config.vm.provision :shell, :inline => "cd /home/vagrant/apache-activemq-5.14.1/bin && sudo sh activemq start", run: "always"

end