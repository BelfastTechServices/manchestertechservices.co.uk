mts-site
==============

# install

## git instructions

```
sudo rm -rf mts-site /var/www/mts-ssl
```

### git url

```
eval $(ssh-agent -s); ssh-add .ssh/github_rsa
git clone git@github.com:BelfastTechServices/mts-site.git
sudo cp -r mts-site/mts/ /var/www/mts-ssl; sudo chown www-data -R /var/www
```

# Setup - multiple domains/ssl sites

```
nano mts-site/mts-ssl.conf
sudo cp mts-site/mts-ssl.conf /etc/apache2/sites-available/mts-ssl.conf; sudo a2ensite mts-ssl; sudo service apache2 restart
sudo certbot --apache -d manchestertechservices.co.uk -d www.manchestertechservices.co.uk -d mta-sts.manchestertechservices.co.uk --agree-tos --renew-by-default --no-redirect
```

## set up MySQL database

```
sudo mysql -u root
CREATE DATABASE `mts-site` CHARACTER SET utf8 COLLATE utf8_general_ci;
CREATE USER 'mts-site'@'localhost' IDENTIFIED WITH mysql_native_password BY '';
GRANT ALL PRIVILEGES ON `mts-site` . * TO 'mts-site'@'localhost';
FLUSH PRIVILEGES;
quit
```

## wget instructions

```
sudo rm -rf latest.tar.gz wordpress /var/www/mts-ssl
wget https://wordpress.org/latest.tar.gz; tar -xzvf latest.tar.gz
nano wordpress/wp-config-sample.php; cp wordpress/wp-config-sample.php wordpress/wp-config.php
sudo cp -r wordpress/ /var/www/mts-ssl; sudo chown www-data -R /var/www
sudo cp -r mts-site/mts/.well-known/ /var/www/mts-ssl/.well-known; sudo chown www-data -R /var/www
git clone https://github.com/psignoret/aad-sso-wordpress.git
sudo cp -r aad-sso-wordpress/ /var/www/mts-ssl/wp-content/plugins/
```
