---
title: les différents FLOW avec Git
prev: /mesidees/19-git/
next: /mesidees/99-trucsalire/

weight: 120
---

### Modèle 1 : a-successful-git-branching-model

sources : 

* [blog d'origine] (http://nvie.com/posts/a-successful-git-branching-model/)
* [comment gérer plusieurs version] (https://blog.nathanaelcherrier.com/2016/08/03/gerer-plusieurs-versions-avec-gitflow/)

##### Liste des branches :
* master => la branche déployable en production à tout instant
* develop => la branche contenant tous les développements réalisés pour la prochaine release
* feature_XX => la branche contenant les commits d'une fonctionnalités
  * initialisée depuis : DEVELOP
  * peut être mergée dans : DEVELOP
* release_XX => la branche qui contient les commits d'une version précise de l'application
 
##### Règles à respecter :

* master :
  * tout commit sur le MASTER donne lieu à un tag et un déploiement en production
* develop :
  * tout développement terminé doit être mergé sur la branche DEVELOP
* feature_XX :
  * à créer depuis DEVELOP
  * à merger dans DEVELOP
  * à ne jamais pousser sur le repository ORIGIN
  * les développeurs peuvent se synchroniser entre eux pour travailler sur une même feature
  * ne pousser dans DEVELOP que quand la fonctionnalité est terminée et souhaitée pour la prochaine version (car la branche de la prochaine version sera créée depuis DEVELOP)
* release_XX :
  * à créer depuis DEVELOP
  * à merger dans DEVELOP et MASTER
  * quand la plus part des fonctionnalités (voire toutes) ont été intégrées dans DEVELOP, on créer la branche release_XX
  * quand la release est terminée et validée, la branche est poussées dans MASTER (pour livraison) et dans DEVELOP (pour y mettre les correctifs)
  * toute correction d'une fonctionnalité doit être faite sur la branche release_XX
* hotFix_XX :	
  * à créer depuis MASTER
  * à merger dans DEVELOP et MASTER
  * si un bug bloquant survient, il est corrigé dans une branche hotFix_XX
  * une fois le bug corrigé, la branche hotFix_XX est poussée dans MASTER et DEVELOP

##### Avantages et problèmes :
* ~ flow complexe mais un outil existe pour en simplifier l'usage (cf. paragraphe suivant)
* - comment vérifier à chaque instant que les branches feature_XX sont intégrables sans conflit ni régression (chacune avec DEVELOP et toutes les combinaisons de feature_XX avec DEVELOP) ?
* + permet des relectures de code facilement directement sur la branche feature_XX avant son intégration dans DEVELOP
* - habituellement, MASTER est la branche centrale d'un dépot. Autant renommer DEVELOP en MASTER et la notion de MASTER dans ce flow n'apporte rien car chaque commit dans MASTER donne lieu à une branche release_XX
* ~ qu'entend-on par feature ? Si une branche feature_XX a une durée de moins de 5 jours et qu'on démarre 4-5 branches le même jour (au début du sprint), les conflits risquent de poser problèmes.
* - ce modèle part du principe qu'il n'existe qu'une version finale à une instant T (le master). Or, dans une prestation de développement d'application par une SSII chez un client, l'équipe gère une version en qualification interne, une en recette client et une en production (modèle un peu ancien mais encore très vivant). Exemple : On a une version 1.0.3 en production et une 1.1 en "recette client". Donc le master est à l'image de la 1.1. Comment créer un patch sur la vielle version ???
 
### Modèle 1 : Git Flow, l'outil qui va avec

sources :

* [introduction à GitFlow] (https://jeffkreeftmeijer.com/git-flow/)
* [Git Flow] (https://blog.nathanaelcherrier.com/2016/07/11/gitflow-la-methodologie-et-la-pratique/)
* [cheat sheet] (https://danielkummer.github.io/git-flow-cheatsheet/index.fr_FR.html)
* [documentation] (https://github.com/petervanderdoes/gitflow-avh)

Pour simplifier l'usage de ce modèle, GitFlow propose de nouvelles commandes qui font les merges, les créations de branche et les cheryPick :

* sur le même modèle que la console GIT, GitFlow permet d'initialiser un repos et notamment de nommer toutes les branches du modèle : ```ssh git flow init```
* pour créer et terminer une feature_XX : ```ssh git flow feature start maFonctionnalité``` et ```ssh git flow feature finish maFonctionnalité```
* pour créer et terminer une release : ```ssh git flow release start 0.1.0``` et ```ssh git flow release finish 0.1.0```

Pour traiter le problème "multi-version" du modèle, gitFlow propose une fonction ```ssh git flow support```. Mais la gestion des cherryPick des bugFixes est à faire manuellement !
 
##### Avantages et problèmes :

* + l'outil simplifie beaucoup l'usage du modèle
* - L'usage de la fonction ```ssh git flow hotfix start``` est impossible (au risque de livrer la 1.1 en production directement).
 
### Modèle 2 :
************
source : 

* [critique du modèle 1] (https://barro.github.io/2016/02/a-succesful-git-branching-model-considered-harmful/)

##### Liste des branches :
* master => la branche qui accueille tous les commit de développement
* release_XX => une branche par livraison
* branchesLocales => des branches que créent les développeurs mais qu'ils ne partagent jamais sur ORIGIN

##### Règles à respecter :
* master :
  * tout les développements sont commités dans le MASTER
* release_XX :
  * la branche release_XX est créée au moment de la livraison de l'application (en même temps que le tag)
  * toute correction d'un bug bloquant se fait sur la branche release_XX
  * toute correction d'un bug bloquant donne lieu à un CHERRY PICK vers le MASTER
* branchesLocales :
  * les branches locales sont une bonne chose à utiliser
  * mais elles doivent être très régulièrement mise à jour vis à vis du MASTER

##### Avantages et problèmes :
* + flow simple
* - pas de relecture de code possible directement depuis un outil centralisé car les branches locales sont "locales"
 
### Annexes :
sources de réflexion :

* [comparaison de workflow] (https://www.atlassian.com/git/tutorials/comparing-workflows#forking-workflow)