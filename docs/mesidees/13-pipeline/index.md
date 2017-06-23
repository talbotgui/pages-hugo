---
title: Pipeline
prev: /mesidees/12-eclipse/
next: /mesidees/14-qualimetrie/

weight: 113
---

* [Définition de l'intégration continue](#définition-de-l-intégration-continue-ic-dans-la-suite-du-chapitre)
* [Les actions à faire réaliser par l'IC](#les-actions-à-faire-réaliser-par-l-ic)
* [Comment aller un tout petit peu plus loin](#comment-aller-un-tout-petit-peu-plus-loin)
* [Définition du Continuous Delivery](#définition-du-continuous-delivery)

## Définition de l'Intégration Continue (IC dans la suite du chapitre)

#### En une seule phrase
**L'intégration continue a pour objectif de valider que le code actuellement présent dans le repository de source est valide.**

La fréquence de la validation et même la définition de cette validation sont très variables d'un projet à un autre :

* la fréquence peut être :
  * 1 fois par jour (minimum du minimum)
  * 2 fois par jour (à 13h et à 23h par exemple)
  * quelques fois par jour (toutes les 2 heures par exemple)
  * à chaque commit (solution optimum)
* la validation peut comprendre :
  * la compilation et le packaging de la solution (minimum du minimum)
  * l'exécution des tests (unitaire & de développement & intégration & bout en bout & non régression)
  * l'analyse de la qualité du code (Sonar par exemple)
  * les tests de performance (non pas pour valider les exigences de performance ou robustesse mais pour valider que les temps d'exécution des fonctionnalités importantes ne se dégradent pas pour un jeu de données et une plateforme donnée)
  * les tests de sécurité (lister les failles potentielles à partir des dépendances de la solution)

#### Pourquoi ? Pour qui ?
L'erreur est humaine. Un développeur peut, par inadvertance et très rarement (bien entendu), pousser sur le repository du code invalide (qui ne compile pas ou qui casse des tests).

A cet instant-là, le projet n'est plus en mesure de livrer la solution. Ce qui est très gênant pour les projets en MCO dont le délai de livraison d'un correctif n'est que de quelques heures. Et surtout les autres développeurs de l'équipe ne peuvent plus récupérer les modifications poussées sur le repository.

Il faut donc corriger cette situation au plus vite. Mais encore faut-il détecter le problème. **L'Intégration Continue est là pour ça !!**

## Les actions à faire réaliser par l'IC

Si l'IC est capable d'exécuter des tests d'intégration voire des tests de bout en bout, elle est donc capable de déployer la solution sur un environnement. Pourquoi ne pas utiliser ce script/outil pour mettre à disposition des testeurs un bouton déclenchant un déploiement de la dernière version du code valide sur l'environnement de test de leur choix ?

Si l'IC peut déclencher n'importe quel script/outil, pourquoi ne pas automatiser les tâches récurrentes et pénibles comme le déplacement des livrables sur le serveur FTP du client ou l'extraction de la liste des bugs corrigés dans une version de la solution ou extraire la liste des commits ou même générer un bon de livraison ?

## Comment aller un tout petit peu plus loin

Bon, arrivé à ce point, qu'a-t-on :

* pour les développeurs, l'IC vérifie régulièrement la validité du code
* pour les testeurs, l'IC déploie une version de la solution sur l'environnement de leur choix
* pour le responsable de la livraison, l'IC prépare tous les livrables (ou une grande partie)

Mais les analystes fonctionnels, les développeurs et les testeurs ont des interactions plus importantes que ça !

* un analyste ne peut-il pas informer les développeurs que son(ses) exigence(s) sont prêtes à être développées (cf. Definition Of Ready) ?
* un développeur ne peut-il pas prévenir un autre développeur que son code est prêt à être relu/revu ?
* un relecteur ne peut-il pas informer un testeur qu'une (ou plusieurs) exigence(s) est (sont) terminées (cf. Definition Of Done) et prête(s) à être qualifiée(s) ?
* un testeur ne peut-il informer le responsable des versions qu'une (ou plusieurs) exigence(s) est(sont) testée(s) ?

Et pourtant, le référentiel des exigences existe certainement (HP ALM par exemple), le référentiel des tests existe aussi (encore HP ALM par exemple) et l'outil de revu/relecture de code est installé. Il est certainement possible de lier tout ce petit monde.

## Définition du Continuous Delivery

Une fois tous les membres de l'équipe outillés, il est possible de décrire le processus de production de l'équipe par un tuyau dans lequel entre une (ou un petit groupe d') exigence(s) qui enchaîneront une série d'étapes jusqu'à être terminées.

Ce processus dépend totalement de l'équipe mais commence toujours avec une exigence (ou un petit paquet). Donc la première étape est toujours la description de cette (ces) exigence(s). Peuvent suivre, la conception détaillée, le développement, la qualification interne, la recette client, le déploiement en pré-production puis celui en production.

Certaines équipes vont y ajouter de la relecture de code ou des revues de conception ou arrêter leur pipeline à la livraison de leur client (s'ils ne gèrent pas ni la recette ni la production).

Et enfin, chaque exigence ne donnant pas lieu à une mise en production, s'ajouteront des étapes conditionnées par une promotion manuelle : une action manuelle qui autorisera une instance du pipeline à avancer.

Voici un exemple de Pipeline très simple et très singulier : le pipeline de mon projet personnel ['Mariage'](/mesprojets/mariage-guidededev)
![pipeline-jenkinsSimple](/images/pipeline-jenkinsSimple.png)


