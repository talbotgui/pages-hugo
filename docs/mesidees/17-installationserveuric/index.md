---
title: Installation d'un serveur pour une Integration Continue
weight: 117
---


#### Création de la machine :

Créer une machine de type *small* chez Amazon EC2 avec l'OS Ubuntu 17.04 AMD64 (image *AMI : ubuntu/images/hvm-ssd/ubuntu-zesty-17.04-amd64-server-20170720*).

Dans le groupe de sécurité, ouvrir les ports HTTP HTTPS SSH (ouverts à tous)

Modification en janvier 2018 : *medium* avec l'image *AMI ubuntu/images/hvm-ssd/ubuntu-artful-17.10-amd64-server-20180102*

#### Mise à jour de la plateforme :
Se connecter en SSH et exécuter
```sudo apt-get update
sudo apt-get upgrade```

#### Installation de Java et Jenkins :
```
sudo apt-get install default-jdk
	
@see http://pkg.jenkins-ci.org/debian/
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian binary/ >> /etc/apt/sources.list'
sudo apt-get update
sudo apt-get install jenkins
```
 
Pour définir le rootContext de Jenkins, exécuter ```sudo vi /etc/default/jenkins``` et modifier la dernière ligne pour ajouter "--prefix=$PREFIX"

#### Installation apache2
```
sudo apt-get install apache2
sudo a2enmod proxy proxy_http
sudo a2enmod headers
sudo a2enmod ssl
```

Créer le fichier */etc/apache2/sites-available/monHttp.conf* (ne pas oublier le sudo)
```
<VirtualHost *:80>
 ServerAdmin webmaster@localhost
 ServerName me.guillaumetalbot.com
 ServerAlias ci
</VirtualHost>
```

```
sudo a2ensite monHttp
sudo /etc/init.d/apache2 restart
```

#### Installation LetsEncrypt
*@see https://certbot.eff.org/#ubuntutzesty-apache*

**Penser à configurer le nom de domaine pour pointer sur la machine auprès du DNS**
```
sudo apt-get update
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install python-certbot-apache 
sudo certbot --apache
```

#### Limiter les informations fournies par le serveur
Modifier le fichier /etc/apache2/conf-available/security et changer les valeurs :

* ServerTokens Prod
* ServerSignature Off

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
```
sudo /etc/init.d/apache2 restart
sudo rm /var/www/html/index.html
```

Créer le fichier /var/www/html/index.html avec le contenu suivant :
```
<!DOCTYPE html>
<html>
 <head>
  <meta charset="utf-8" />
  <meta http-equiv="refresh" content="3; url=https://talbotgui.github.io" />
 </head>
 <body>
  Redirection en cours...
 </body>
</html>
```

#### Configuration Jenkins :
Aller à l'adresse https://...../jenkins et suivre les instructions

#### Installation email : 
```
sudo apt-get install postfix
sudo vi /etc/postfix/main.cf
  myhostname = guillaumetalbot.com
sudo vi /etc/postfix/virtual
  webmaster@guillaumetalbot.com ubuntu
sudo postmap /etc/postfix/virtual
sudo service postfix restart
```

#### Installation Maven :
```sudo apt-get install maven```
Modifier le fichier /usr/share/maven/conf/settings.xml pour définir le repo local sans oublier de créer le répertoire avec le droit à tous les utilisateurs (chmod a+w m2repo)

#### Installation NPM & NodeJS
```
curl -sL https://deb.nodesource.com/setup_9.x | sudo -E bash -
sudo apt-get install -y nodejs
```

#### Installation de Chrome :
```
sudo sh -c 'echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'
wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
sudo apt-get update
sudo apt-get install google-chrome-stable
```

#### Si la machine est de type *tiny*
La machine va manquer de RAM. Pour ajouter une partition de SWAP (dans un fichier) (http://tecadmin.net/add-swap-partition-on-ec2-linux-instance/)
```
dd if=/dev/zero of=/var/myswap bs=1M count=2048
mkswap /var/myswap
swapon /var/myswap
```

#### Les répertoires à vider pour récupérer de l'espace :
/var/lib/jenkins/.npm/_logs/


#### Notes sur l'installation d'un Archiva
Ressources :

* documentation : https://archiva.apache.org/docs/2.2.3/quick-start.html
* installation : http://apache.mirrors.nublue.co.uk/archiva/2.2.3/binaries/apache-archiva-2.2.3-bin.zip

Installation :

* extraire le contenu de l'archive
* si Java9 est le JDK par défaut, modifier le fichier conf/wrapper.conf : ``` wrapper.java.command=C:/Program Files/Java/jdk1.8.0_151/bin/java ```
* sous Windows, dans un interpréteur de commande en mode "Admin", exécuter ``` archiva install ``` puis ``` archiva start ```
* Archiva sera disponible à l'adresse http://localhost:8080/

#### Petites commandes pratiques pour entretenir le serveur :

Pour rechercher les répertoires prenant de la place : ```sudo du -cha --max-depth=1 / | grep -E "[0-9]M|[0-9]G"```

Et un petit script permettant de faire le ménage régulièrement :
```
apt clean all
rm /var/log/**/*.gz
rm /var/log/journal/*/*-00000*.journal
rm -rf /var/lib/jenkins/workspace/*
rm -rf /var/lib/jenkins/.sonar/*
rm -rf /var/lib/jenkins/.m2/*
rm -rf /var/lib/jenkins/.npm/*
```

#### Installation du Cloud SDK

* documentation : https://cloud.google.com/sdk/docs/downloads-apt-get

Installation :
```
export CLOUD_SDK_REPO="cloud-sdk-$(lsb_release -c -s)"
echo "deb http://packages.cloud.google.com/apt $CLOUD_SDK_REPO main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
sudo apt-get update && sudo apt-get install google-cloud-sdk
```

Connexion :

* créer une clef depuis la console GCloud (IAM / comptes de service)
* activer les APIs "Cloud Resource Manager API" et "App Engine Admin API" depuis la console GCloud (API / Bibliotheque)
* déposer le fichier JSON sur le serveur
* se connecter une première fois avec ```gcloud auth activate-service-account --key-file=LE_FICHIER_JSON```
* exécuter la commande ```gcloud init``` et renseigner toutes les informations
