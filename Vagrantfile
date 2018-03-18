# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "ubuntu/trusty64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
  config.vm.network "forwarded_port", guest: 9000, host: 9000, id: "SonarQube-Web"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    # vb.gui = true

    # Customize the amount of memory on the VM:
    vb.memory = "2048"
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    log () {
      echo "$(date): $@"
    }

    log "Downloading JVM..."

    if [ ! -f jdk-8u161-linux-x64.tar.gz ]; then
      wget --no-verbose -c --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u161-b12/2f38c3b165be4555a1fa6e98c45e0808/jdk-8u161-linux-x64.tar.gz
    else
      log "File jdk-8u161-linux-x64.tar.gz already exists. Skipping download."
    fi

    echo "6dbc56a0e3310b69e91bb64db63a485bd7b6a8083f08e48047276380a0e2021e  jdk-8u161-linux-x64.tar.gz" > jdk-8u161-linux-x64.tar.gz.sha256

    if ! sha256sum -c jdk-8u161-linux-x64.tar.gz.sha256; then
        log "$(date): JVA sha256sum doesn't match"
        log "$(date): Exiting..."
        exit 2
    fi

    tar -xzf jdk-8u161-linux-x64.tar.gz

    log "Downloading SonarQube..."

    if [ ! -f sonarqube-7.0.zip ]; then
      wget --no-verbose https://sonarsource.bintray.com/Distribution/sonarqube/sonarqube-7.0.zip
    else
      log "File sonarqube-7.0.zip already exists. Skipping download."
    fi

    echo "a9e01118beddf1cc60d3c1f126e77d85  sonarqube-7.0.zip" > sonarqube-7.0.zip.md5

    if ! md5sum -c sonarqube-7.0.zip.md5; then
        log "Sonarqube md5 doesn't match"
        log "Exiting..."
        exit 2
    fi

    sudo apt-get -y install unzip
    unzip -q sonarqube-7.0.zip

    log "Updating the SonarQube configuration..."
    if [ ! -f sonarqube-7.0/conf/wrapper.conf.original ]; then
      cp sonarqube-7.0/conf/wrapper.conf sonarqube-7.0/conf/wrapper.conf.original
    fi
    sed -i 's~wrapper.java.command=java~wrapper.java.command=/home/vagrant/jdk1.8.0_161/bin/java~' sonarqube-7.0/conf/wrapper.conf
    sed -i 's~XX#RUN_AS_USER=~RUN_AS_USER=vagrant~' sonarqube-7.0/bin/linux-x86-64/sonar.sh
  SHELL

  # Start the sonarqube web server
  config.vm.provision "shell", privileged: false, run: 'always', inline: <<-SHELL
    sonarqube-7.0/bin/linux-x86-64/sonar.sh start
  SHELL

end
