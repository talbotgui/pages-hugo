---
title: Créer un compte sftp sur AWS
prev: /mesidees/23-gitetclefssshmultiples/
next: /mesidees/25-featuretoggle/

weight: 124
---

### Créer le nouvel utilisateur
Exécuter les commandes :

* ```sudo adduser new_user --disabled-password```
* ```sudo su new_user```
* ```cd```
* ```mkdir .ssh```
* ```chmod 700 .ssh```
* ```touch .ssh/authorized_keys```
* ```chmod 600 .ssh/authorized_keys```

### Sécuriser l'accès
* Créer une paire de clef sur la console EC2 d'AWS
* Uploader la clef sur le serveur dans /home/ubuntu
* La transformer en clef publique RSA : ```ssh-keygen -f /home/ubuntu/xxx.pem -y >/home/new_user/.ssh/authorized_keys```

