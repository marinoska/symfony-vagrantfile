# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

    config.vm.box = "scotch/box"
    config.vm.network "private_network", ip: "192.168.33.10"
    config.vm.hostname = "scotchbox"
    config.vm.network "forwarded_port", guest: 80, host: 8080
    config.vm.network :forwarded_port,  guest: 8081,  host: 8081
    # mysql
    config.vm.network :forwarded_port,  guest: 3306,  host: 3306
    # mongo
    config.vm.network :forwarded_port,  guest: 27017, host: 27777
    config.vm.synced_folder ".", "/var/www", :mount_options => ["dmode=777", "fmode=666"]

    config.vm.provider "virtualbox" do |v|
        v.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/var_www", "1"]
    end

    config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get upgrade
	sudo apt-get install curl libcurl3 libcurl3-dev php5-curl php5-xdebug
	sudo service apache2 restart

    sudo sed -i s,/var/www/public,/var/www/web,g /etc/apache2/sites-available/000-default.conf
    sudo sed -i s,/var/www/public,/var/www/web,g /etc/apache2/sites-available/scotchbox.local.conf
    sudo service apache2 restart

    # Add xdebug settings in the *.ini file
    echo "zend_extension=xdebug.so" >> /etc/php5/mods-available/xdebug.ini
    echo "xdebug.remote_enable=true" >> /etc/php5/mods-available/xdebug.ini
    echo "xdebug.remote_port = 9000" >> /etc/php5/mods-available/xdebug.ini
    echo "xdebug.remote_host = 192.168.33.1" >> /etc/php5/mods-available/xdebug.ini
    SHELL

end
