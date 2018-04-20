---
title: Git et clefs SSH multiples
weight: 123
---

### Cas d'usage
Quand on développe à la maison et au bureau avec Git, rapidement, apparait le besoin de disposer de plusieurs clefs SSH pour s'authentifier auprès des serveurs.

### Etapes :

* Créer les clefs (en reprenant le chemin complet du fichier proposé !!) :
 * ```ssh-keygen -t rsa -C "premier@email.com"``` avec /c/Users/xxxx/.ssh/id_rsa_boulot
 * ```ssh-keygen -t rsa -C "second@email.com"``` avec /c/Users/xxxx/.ssh/id_rsa_perso


* Créer le fichier ~/.ssh/config avec 
```
# Compte perso
Host github.com
  HostName github.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_rsa_perso
# Compte boulot
Host qsd.mon.boulot
  HostName qsd.mon.boulot
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_rsa_boulot
```

* Pour déclarer les clefs :
```ssh-add ~/.ssh/id_rsa_perso```
```ssh-add ~/.ssh/id_rsa_boulot```

* Pour démarrer l'agent SSH (ou s'assurer qu'il fonctionne)
```eval `ssh-agent -s` ```

* Supprimer le cache des clefs
```ssh-add -D```

Pour tester (si besoin, accepter la connexion en répondant 'yes') :
```ssh -T git@github.com```
```ssh -T git@qsd.mon.boulot```

	
ET NE PAS OUBLIER DE FAIRE DES CLONES EN UTILISANT LE PROTOCOLE SSH et non https !!!!