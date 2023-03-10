# Vagrant

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Vagrant images: [https://app.vagrantup.com/boxes/search](https://app.vagrantup.com/boxes/search)

* vagrant init - create vagr ant file
* vagrant up - start vm
* vagrant halt - stop vm
* vagrant ssh - ssh to vm
* vagrant destroy - delete vm

## Vagrant File

config.vm.network "private\_network", ip: "192.168.33.10" - to set static IP

config.vm.network "public\_network" - dynamic IP (bridged adaptor)

config cpu, memory:

```ruby
config.vm.provider "virtualbox" do |v|
  v.memory = 1024
  v.cpus = 2
end
```

Sync directories: first location is on your local device, second on linux device

`config.vm.synced_folder "D:\shells_scripts", "/vagrant_data"`

<figure><img src=".gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

### Website setup with html template

* install services such as httpd, wget, unzip
* /etc/httpd/conf/httpd.conf - apache config file. Usefull information there
* /var/www/html/index.html - file to replace  default httpd website
* download website template and unzip it ([https://www.tooplate.com](https://www.tooplate.com) for templates)
* copy all od the content to replace default html: cp -r \* /var/www/html/
* systemctl restart httpd

or the same using vagrant file:

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

### Wordpress install Ubuntu

* Follow the official install guide: [https://ubuntu.com/tutorials/install-and-configure-wordpress#1-overview](https://ubuntu.com/tutorials/install-and-configure-wordpress#1-overview)

### Wordpress vagrant file (IAAC)

```
config.vm.provision "shell", inline: <<-SHELL
     sudo apt update
     sudo apt install apache2 \
                         ghostscript \
                         libapache2-mod-php \
                         mysql-server \
                         php \
                         php-bcmath \
                         php-curl \
                         php-imagick \
                         php-intl \
                         php-json \
                         php-mbstring \
                         php-mysql \
                         php-xml \
                         php-zip -y
    sudo mkdir -p /srv/www
    sudo chown www-data: /srv/www
    curl https://wordpress.org/latest.tar.gz | sudo -u www-data tar zx -C /srv/www
 # Create file on your PC and copy it to VM since you can't do it directly in vagrant file  
    cp /vagrant/wordpress.conf /etc/apache2/sites-available/wordpress.conf
    sudo a2ensite wordpress
    sudo a2enmod rewrite
    sudo a2dissite 000-default
    sudo service apache2 reload
 # Run mysql cmf from shell
    mysql -u root -e 'CREATE DATABASE wordpress;'
    mysql -u root -e 'CREATE USER wordpress@localhost IDENTIFIED BY "dbpass";'
    mysql -u root -e 'GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER ON wordpress.* TO wordpress@localhost;'
    mysql -u root -e 'FLUSH PRIVILEGES;'
    
    sudo -u www-data cp /srv/www/wordpress/wp-config-sample.php /srv/www/wordpress/wp-config.php
    sudo -u www-data sed -i 's/database_name_here/wordpress/' /srv/www/wordpress/wp-config.php
    sudo -u www-data sed -i 's/username_here/wordpress/' /srv/www/wordpress/wp-config.php
    sudo -u www-data sed -i 's/password_here/dbpass/' /srv/www/wordpress/wp-config.php
   SHELL
```

