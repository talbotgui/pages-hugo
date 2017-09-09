---
title: Installation d'un serveur pour une Integration Continue
prev: /mesidees/16-angular/
next: /mesidees/18-licences/

weight: 117
---


#### Création de la machine :

Créer une machine de type *small* chez Amazon EC2 avec l'OS Ubuntu 17.04 AMD64 (image *AMI : ubuntu/images/hvm-ssd/ubuntu-zesty-17.04-amd64-server-20170720*).
Dans le groupe de sécurité, ouvrir les ports HTTP HTTPS SSH (ouverts à tous)

#### Mise à jour de la plateforme :
Se connecter en SSH et exécuter
```ssh sudo apt-get update
sudo apt-get upgrade```

#### Installation de Java et Jenkins :
```ssh	
sudo apt-get install default-jdk
	
@see http://pkg.jenkins-ci.org/debian/
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian binary/ >> /etc/apt/sources.list'
sudo apt-get update
sudo apt-get install jenkins
```
 
Pour définir le rootContext de Jenkins, exécuter ```ssh sudo vi /etc/default/jenkins``` et modifier la dernière ligne pour ajouter "--prefix=$PREFIX"

#### Installation apache2
```ssh
sudo apt-get install apache2
sudo a2enmod proxy proxy_http
sudo a2enmod headers
sudo a2enmod ssl
```

Créer le fichier */etc/apache2/sites-available/monHttp.conf*
```
<VirtualHost *:80>
 ServerAdmin webmaster@localhost
 ServerName me.guillaumetalbot.com
 ServerAlias ci
</VirtualHost>
```

```ssh
sudo a2ensite monHttp
sudo /etc/init.d/apache2 restart
```

#### Installation LetsEncrypt
*@see https://certbot.eff.org/#ubuntutzesty-apache*

**Penser à configurer le nom de domaine pour pointer sur la machine auprès du DNS**
```ssh
sudo apt-get update
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install python-certbot-apache 
sudo certbot --apache
```

#### Activer les accès sécurisés aux outils :
Modifier le fichier /etc/apache2/sites-available/monHttp-le-ssl.conf et y ajouter
```
DocumentRoot /var/www/html
ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined
ProxyRequests Off
ProxyVia Off
<Proxy *>
Order deny,allow
Allow from all
</Proxy>
ProxyPreserveHost on
ProxyPass /monMariage http://localhost:9090/monMariage
ProxyPassReverse /monMariage http://locahost:9090/monMariage
ProxyPass /jenkins http://localhost:8080/jenkins nocanon
ProxyPassReverse /jenkins http://locahost:8080/jenkins
ProxyPassReverse /jenkins  http://me.guillaumetalbot.com/jenkins
RequestHeader set X-Forwarded-Proto "https"
RequestHeader set X-Forwarded-Port "443"
AllowEncodedSlashes NoDecode

<IfModule mod_headers.c>
 Header set X-Content-Type-Options nosniff
 Header set Content-Security-Policy "child-src 'none'; object-src 'none'"
 Header set X-Permitted-Cross-Domain-Policies "none"
 Header set X-Frame-Options DENY
 Header set X-XSS-Protection "1; mode=block;"
 Header set Strict-Transport-Security "max-age=300; includeSubDomains; preload; always;"
 Header set Public-Key-Pins "pin-sha256=\"base64+primary==\"; pin-sha256=\"base64+backup==\"; max-age=5184000; includeSubDomains"
</IfModule>

```

Puis exécuter :
```ssh
sudo /etc/init.d/apache2 restart
sudo rm /var/www/html/index.html
```

Créer le fichier /var/www/html/index.html avec le contenu suivant :
```html
<!DOCTYPE html>
<html>
 <head>
  <meta charset="utf-8" />
  <meta http-equiv="refresh" content="0; url=https://me.guillaumetalbot.com/monMariage/login.html" />
 </head>
 <body>
  Redirection en cours
 </body>
</html>
```

#### Configuration Jenkins :
Aller à l'adresse https://...../jenkins et suivre les instructions

#### Installation email : 
```ssh
sudo apt-get install postfix
sudo vi /etc/postfix/main.cf
  myhostname = guillaumetalbot.com
sudo vi /etc/postfix/virtual
  webmaster@ec2-52-49-145-20.eu-west-1.compute.amazonaws.com ubuntu
sudo postmap /etc/postfix/virtual
sudo service postfix restart
```

#### Installation Maven :
```ssh sudo apt-get install maven```
Modifier le fichier /usr/share/maven/conf/settings.xml pour définir le repo local sans oublier de créer le répertoire avec le droit à tous les utilisateurs (chmod a+w m2repo)

#### Installation NPM & NodeJS
```ssh
curl -sL https://deb.nodesource.com/setup_7.x | sudo bash -
sudo apt-get install nodejs
```

#### Installation de Chrome :
```ssh
sh -c 'echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'
wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
sudo apt-get update
sudo apt-get install google-chrome-stable
```

#### Si la machine est de type *tiny*
La machine va manquer de RAM. Pour ajouter une partition de SWAP (dans un fichier) (http://tecadmin.net/add-swap-partition-on-ec2-linux-instance/)
```ssh
dd if=/dev/zero of=/var/myswap bs=1M count=2048
mkswap /var/myswap
swapon /var/myswap
```

#### Les répertoires à vider pour récupérer de l'espace :
/var/lib/jenkins/.npm/_logs/