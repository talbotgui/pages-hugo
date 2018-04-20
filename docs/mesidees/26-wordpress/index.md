---
title: Installer une instance WordPress sur Ubuntu
weight: 126
---

### Prérequis
Créer un serveur Ubuntu de type TINY sur Amazon Web Services avec un serveur httpd (cf. https://talbotgui.github.io/mesidees/17-installationserveuric/)

### Installer les outils

* MySQL : ```sudo apt-get install mysql-server```
* PHP et ses librairies httpd : 
 * ```sudo apt-get install php libapache2-mod-php php-mysql```
 * ```sudo apt-get install php-curl php-gd php-mbstring php-mcrypt php-xml php-xmlrpc```
 * ```sudo /etc/init.d/apache2 restart```

### Créer la base de données :

* se connecter à la base : ```mysql -u root -p```
* puis exécuter les commandes :
 * ```CREATE DATABASE wordpress DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;```
 * ```GRANT ALL ON wordpress.* TO 'wordpressuser'@'localhost' IDENTIFIED BY 'password';```
 * ```FLUSH PRIVILEGES;```
 * ```EXIT;```

### Configurer Apache :

* Configurer Apche ```sudo vi /etc/apache2/apache2.conf```
* Ajouter la balise ```AllowOverride All``` dans <Directory /var/www/html/>
* Activer le mode rewrite ```sudo a2enmod rewrite```
* Redémarrer Apache ```sudo /etc/init.d/apache2 restart```

### Installer WordPress :

* ```mkdir tmp```
* ```cd tmp```
* ```curl -O https://wordpress.org/latest.tar.gz```
* ```tar xzvf latest.tar.gz```
* ```touch wordpress/.htaccess```
* ```chmod 660 wordpress/.htaccess```
* ```cp wordpress/wp-config-sample.php wordpress/wp-config.php```
* ```mkdir wordpress/wp-content/upgrade```
* ```sudo cp -a wordpress/. /var/www/html```
* ```sudo chown -R sammy:www-data /var/www/html```
* editer /var/www/html/.htaccess pour y mettre les lignes :
 * ```php_value upload_max_filesize 64M```
 * ```php_value post_max_size 128M```
* ```curl -s https://api.wordpress.org/secret-key/1.1/salt/```
* éditer le fichier /var/www/html/wp-config.php
 * y remplacer les lignes d'exemple par les lignes issue de la requête
 * paramétrer la base de données
* ```sudo chown -R www-data:www-data /var/www/html```
* ```sudo find /var/www/html -type d -exec chmod g+s {} \;```
* ```sudo chmod g+w /var/www/html/wp-content```
* ```sudo chmod -R g+w /var/www/html/wp-content/themes```
* ```sudo chmod -R g+w /var/www/html/wp-content/plugins```
