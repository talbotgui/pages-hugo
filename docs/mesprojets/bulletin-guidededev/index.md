---
title: Bulletin NG - Guide du développeur
prev: /mesprojets/mariage-guidededev/
next: /moncv/
weight: 201

---

Cette page est mini guide du développeur ([ma définition du guide du développeur](/mesidees/10-guidedudev/)) pour le projet [BulletinNg] (https://github.com/talbotgui/bulletin-ng).

**Sommaire** :

* [Gestion de projet] (#gestion-de-projet)
* [Exigences] (#exigences)
  * [Exigences métiers] (#métiers)
  * [Exigences techniques] (#techniques)
* [Environnement] (#environnement)
* [Architecture technique] (#architecture-technique)
* [Tests] (#tests)
  * [Que tester] (#que-tester)
  * [Comment tester] (#comment-tester)
* [Règles de conception] (#règles-de-conception)
  * [Données] (#données)
  * [Service] (#service)
  * [Composants Angular] (#composants-angular)
* [Règles de développement] (#règles-de-développement)
  * [Langue] (#langue)
  * [Nommage] (#nommage)
  * [Mise en forme des sources] (#mise-en-forme-des-sources)
* [Processus de développement] (#processus-de-développement)
  * [Gestion de configuration logicielle] (#gestion-de-configuration-logicielle-gcl)
  * [Commit] (#commit)
  * [Industrialisation] (#industrialisation)


## Gestion de projet
Aucun suivi des tâches n'est en cours pour le moment car les demandes arrivent et sont traitées au fil de l'eau.

Sont présents en tâches de fond les éléments suivants :

* mettre en oeuvre un maximum des fonctionnalités de Angular, AngularMaterial et des outils de build (NPM, Karma, Jasmine, Protractor, ...).

## Exigences
Les exigences sont actuellement fournies par la principale utilisatrice. Aucune formalisation n'existe en dehors des quelques lignes ci-dessous.

#### Métiers
Du point de vue métier, le besoin est très spécifique et ne sera pas détaillé. Les principales contraintes métiers ayant amener à des choix techniques discutables sont :

* le caractère privé des données nécessite que ces dernières ne soient pas stockées dans une base de données centralisées. L'utilisateur a besoin d'avoir une maitrise complète de leur stockage.
  * les données sont donc stockées dans un fichier uploadé et téléchargé par l'utilisateur au début et à la fin de sa session
  * les utilisateurs disposant d'un serveur de stockage de données (NAS Network Attached Storage) peuvent l'utiliser pour sauvegarder et charger leurs données (à eux de sécuriser leur serveur)
  * l'application ne doit réaliser aucun transfert de données (sauf dans le cas de la sauvegarde sur un serveur qui est le résultat d'une action explicite de l'utilisateur)
* l'application doit être sécurisée dans la mesure du possible pour éviter l'injection de script qui pourrait tenter de voler les données

#### Techniques

Du point de vue technique, l'application doit être 

* l'accessibilité n'est pas un besoin car l'utilisateur doit avoir ses données sur son terminal ;
* évolutive : chaque nouveau besoin doit être facilement réalisé pour fournir le service rapidement ;
* fiable (point primordiale) : l'application ne doit pas subir de régressions intempestives portant atteinte à son utilisabilité ou à la fiabilité de ses données ;
* la structure des données sauvegardées ne doit pas être modifiée afin de conserver la rétrocompatibilité avec la précédente application.

## Environnement
Ce projet ne compte pas tous les environnements d'un projet classique.

(cf. [chapitre Environnement du guide du développeur du projet Mariage] (https://talbotgui.github.io/mesprojets/mariage-guidededev/#environnement))

## Architecture technique

Pour limiter les transferts réseau, l'application se résume à un FrontEnd. Afin de simplifier le code et d'en augmenter la maintenabilité, le développement n'est pas réalisé en JS/JQuery mais avec Angular (très bon framework pour faire du binding données/HTML)

Les tests sont cruciaux dans le cadre de cette application. Donc, sans aller jusqu'à une couverture de code de 100%/100% (en ligne / en branche), toutes les fonctionnalités doivent être testées (cf. paragraphe dédié aux tests).

## Tests
#### Que tester
Les cas nominaux de tous les composants doivent être testés ainsi que les cas d'erreur principaux.

Aucune exigence n'impose une couverture de code particulière mais 80% est souhaitable pour tous les composants contenant de la logique (les services).

#### Comment tester
Jasmine et Ts6-Mockito sont utilisés pour les tests unitaires des services.

Protractor est utilisé pour les tests de bout en bout (e2e - end to end).

## Règles de conception
#### Données
Le modèle de données ne doit pas évoluer au point de briser la rétrocompatibilité avec l'application précédente.
#### Service
Toute logique ou manipulation de données doit se faire dans un service.

La manipulation du singleton de données est de la responsabilité de DataRepository (ce qui facilite les tests unitaires en offrant un point d'entrée à un bouchon).

Les lectures ou agrégations de données doivent se faire dans LectureService.

Les manipulations complexes, non réutilisables et propres à un sous-ensemble du jeu de données doivent être placés dans un service dédié.

#### Composants Angular
Les composants doivent être les plus simples possibles et limités la duplication de code. Pour cela, les éléments communs à tous les écrans comme la saisie de note ou de compétence doivent être l'objet de composant dédié.

## Règles de développement
Les règles ci-dessous doivent être appliquées par tout développeur souhaitant participer à ce projet (non négociable). **Par contre, il est toujours possible de discuter de la modification d'une règle.**

#### Langue
Les entités, les attributs et les méthodes sont nommés en français.
Les préfixes, suffixes et concepts inhérents au langage reste en anglais (get, set, is, ...)

#### Nommage
Les composants applicatifs sont suffixés selon les règles définies par Angular : 

* "xx.service.ts" pour les service (sauf l'unique exception de DataRepository) ;
* "xx.component.ts" pour le code TypeScript des composants d'IHM ;
* "xx.component.html" pour le code HTML des composants d'IHM ;
* "xx.component.css" pour les styles des composants d'IHM ;
* et "xx.spec.ts" pour les tests.

Parmi les composants d'IHM, un préfixe est ajouté en fonction de l'usage et du type de composant :
* "compo-xxx" pour les composants réutilisables ;
* "div-xxx" pour les cadres présents dans la page et visibles à tout instant ;
* "tab-xxx" pour un onglet.

Les entités métier et les classes utilitaires ne sont pas suffixées mais doivent avoir des noms parlant (l'usage du nom d'un pattern est utile pour décrire l'usage/l'utilité du composant)
Les noms de méthode commence par un verbe à l'infinitif

#### Mise en forme des sources
Microsoft VsCode est l'IDE préconisé même si le plugin pour Eclipse n'est pas mal mais plante encore régulièrement (testé en aout 2017). La configuration VsCode est présente dans le repository et les règles de validation TsLint doivent être respectées !


## Processus de développement
#### Gestion de configuration logicielle (GCL)
* Que comprend la GCL :
  * Les sources applicatives et de test sont dans un repository GIT ;
  * Le présent "guide du développeur" est lui aussi dans un repository GIT mais séparé (pas de gestion de version cohérente nécessaire) ;
  * Le bug tracker a utilisé est celui du repository GitHub ;
  * Les plans de test "métier" ne sont pas formalisés. Mais une partie d'entre eux sont automatisés.
* La gestion de version :
  * une seule version existe pour le moment (branche 'master').
  * si des POC sont réalisés, ils doivent l'être dans une branche dédiée
  * aucun TAG n'est posé pour le moment (c'est une chose à faire prochainement)
  * l'usage de featureBranch est autorisé et même encouragé

#### Commit 
Les messages de commit doivent être explicites.

#### Industrialisation
##### Outils
La gestion des exigences est totalement informelle. Ce qui ne pose pas de problème pour ce projet car le délai entre l'expression du besoin et la mise en production est court et qu'aucune contractualisation n'existe (ni sur les moyens ni sur le périmètre métier).
GitHub héberge une partie des outils du projet : repository GIT & guide du développeur, ...
L'outil d'intégration continue utilisé est Jenkins (hébergé sur un serveur EC2).
La qualimétrie est réalisée avec SonarQube (hébergée sur le site officiel en ligne).
Les relectures de code sont possibles avec GitLab mais ne sont pas utilisées actuellement (un seul développeur pour le moment)

##### Processus
Quitte à faire un petit projet sympa, autant utiliser les outils correctement. Donc le processus de développement (même s'il est minimal) est piloté par un pipeline (cf. [pipeline](/mesidees/13-pipeline/)).
Le pipeline est lui aussi en GCL : dans le *JenkinsFile* 
Le pipeline commence au commit du code et enchaîne les étapes suivantes : compilation, tests métiers, tests E2E, qualimétrie, promotion manuelle et mise en production

