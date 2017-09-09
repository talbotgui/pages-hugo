---
title: Mariage - Guide du développeur
prev: /mesprojets/
next: /mesprojets/bulletin-guidededev/
weight: 201

---

Cette page est un court guide du développeur ([ma définition du guide du développeur](/mesidees/10-guidedudev/)) pour mon projet [Mariage] (https://github.com/talbotgui/mariage).

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
  * [Avec quels outils] (#avec-quels-outils)
* [Règles de conception] (#règles-de-conception)
  * [Persistance] (#persistance)
  * [Service] (#service)
  * [API] (#api)
  * [WEB - Pages] (#web-pages)
  * [WEB - JS] (#web-js)
* [Règles de développement] (#règles-de-développement)
  * [Langue] (#langue)
  * [Nommage] (#nommage)
  * [Mise en forme des sources] (#mise-en-forme-des-sources)
  * [HQL] (#hql)
* [Processus de développement] (#processus-de-développement)
  * [Gestion de configuration logicielle] (#gestion-de-configuration-logicielle-gcl)
  * [Commit] (#commit)
  * [Industrialisation] (#industrialisation)



## Gestion de projet
Un suivi minimaliste des tâches est réalisés dans le fichier "backlog.txt" placé à la racine de la branche "master". Cette backlog est alimentée en fonction des besoins métiers exprimés ou des opportunités techniques.

## Exigences
Les exigences sont actuellement fournies par la principale utilisatrice. Aucune formalisation n'existe en dehors des quelques lignes ci-dessous.

#### métiers
Du point de vue métier, le besoin est assez simple : gérer les données d'un mariage. Cela comprend :

* la gestion de 
  * la liste des invités,
  * la liste des différents courriers, 
* le suivi de
  * l'envoi des courriers, 
  * la réception des réponses, 
* la planification des événements, 
* le calcul des statistiques nécessaires aux mariés et prestataires, 
* le partage de ces informations avec les proches participants à l'organisation (parents, témoins, ...)

Il existe déjà des sites pour faire tout ça. Mais aucun qui nous correspondait exactement. Et tous, systématiquement, sont envahis de publicité (*donc, en bon geek, j'ai fait le mien*).

#### techniques

Du point de vue technique, l'application doit être 

* facilement accessible sur tout support : PC, tablette, téléphone ;
* disponible à tout moment : 24/7 sans contrainte temporelle de redémarrage en cas d'arrêt ;
* évolutive : chaque nouveau besoin doit être facilement réalisé pour fournir le service rapidement à l'utilisatrice principale ;
* fiable (point primordiale) : l'application ne doit pas subir de régressions intempestives portant atteinte à son utilisabilité ou à la fiabilité de ses données.

## Environnement
Ce projet ne compte pas tous les environnements d'un projet classique (comprendre "projet d'une ESN pour un de ses clients"). Comme l'équipe "projet" ne comprend qu'un seul membre et qu'il n'existe qu'une seule version de l'application (pour le moment en tout cas), il n'est pas nécessaire de prévoir plus de 3 environnements (un projet classique en comprend au moins 6) :

* Développement & tests : 
  * pourquoi : il faut bien coder l'application avant de pouvoir l'utiliser.
  * caractéristiques : poste Windows avec un Eclipse (Néon ou supérieur) avec le plugin HibernateTools (cf. [billet dédié](/mesidees/04-consolehibernate/))
  * différences notables avec l'environnement de production : OS Windows vs Linux en production, pas de reverse-proxy HTTPd ni de HTTPs, pas de manipulation du cache par les entêtes HTTP
* Intégration :
  * pourquoi : aucun développeur, même tout seul, n'est infaillible. De temps en temps, une régression peut être introduite ou un défaut de qualité peut échapper à notre vigilance. L'intégration continue est mon garde-fou et nécessite un environnement dédié.
  * caractéristiques : sur la même VM que la production. Donc l'application démarrée durant les tests d'intégration doit démarrer sur un autre port que celle en production. 
  * différences notables avec l'environnement de production : pas de reverse-proxy HTTPd ni de HTTPs, pas de manipulation du cache par les entêtes HTTP
* Production :
  * pourquoi : pour faire fonctionner l'application sur un serveur stable, suffisamment disponible pour répondre aux exigences (cf. ci-dessus) et exposé à Internet
  * caractéristiques : Linux maintenu à jour régulièrement sur une VM hébergée par Amazon (EC2)

## Architecture technique

En bon développeur Java, l'application se base sur Hibernate et Spring. Pour ne pas avoir à installer de composant tiers sur les environnements et parce que les exigences le permettent, l'application est autoporteuse avec SpringBoot. Ce dernier démarre un serveur WEB (Tomcat) et une base de données (HSQL) automatiquement.

Pour réduire les bugs, rien de tel que de réduire le volume de code. Donc SpringData pour la persistance. Ce qui impose l'usage de JPA à la place de l'API Hibernate native.

L'exposition des services en API REST avec SpringMVC (pour continuer avec Spring et se faciliter l'intégration des composants). 

L'application WEB est assez simple (voire simpliste) et se compose principalement de tableaux de données chargées depuis l'API REST. Une application Angular eut été possible mais, vu le besoin, une simple application avec JQuery, JQueryUI et JQxgrid a été retenue.

Les tests sont cruciaux dans le cadre de cette application. Donc, sans aller jusqu'à une couverture de code de 100%/100% (en ligne / en branche), toutes les fonctionnalités doivent être testées sur toutes les couches (cf. paragraphe dédié aux tests).

## Tests
#### Que tester
Les cas nominaux de tous les composants doivent être testés ainsi que les cas d'erreur principaux.

Aucune exigence n'impose une couverture de code particulière mais 80% est souhaitable pour tous les composants contenant de la logique (hors getter, setter, constructeur, code de [programmation défensive](/mesidees/11-programmationdefensive/), ...)

#### Comment tester
**Les composants métiers** (Service, Repostitory et Entité) doivent être testés avec des tests d'intégration. Il n'est pas concevable de tester un système entièrement basé sur la notion de transaction et de requêtes SQL (Spring, Hibernate et la BDD) sans une connexion à une base de données et sans activer les commits.

Si certains composants métiers présentent une grande complexité, alors un [test de granularité unitaire](/mesidees/03-testsdedeveloppement/) est tout à fait envisageable. Un tel composant n'existe pas pour le moment.

**Les contrôleurs REST** doivent être testés unitairement. Ces composants ne sont pas transactionnels et les services sont testés par ailleurs. Mais, pour bien vérifier le paramétrage et le code, il est nécessaire de réaliser le test des contrôleurs en démarrant un serveur WEB (l'adjectif "unitaire" utilisé plus tôt est donc discutable - cf. [test de développement](/mesidees/03-testsdedeveloppement/)).

**Les écrans** de l'application sont testés en mode "bout en bout". Selenium est utilisé pour piloter le navigateur et un jeu de données est utilisé pour initialiser le test. ATTENTION : les tests Selenium ne sont donc pas tous indépendants les uns des autres. Ce n'est pas une bonne pratique mais faire autrement avec un test de bout en bout est trop coûteux.

#### Avec quels outils
**JUnit** et **TestNG** sont tous deux des exécuteurs de tests. JUnit est utilisé dans les tests des composants métiers. TestNG, étant plus pratique à utiliser avec Mockito, est utilisé pour l'exécution des tests des contrôleurs.

**Mockito** est donc utilisé pour bouchonner les services métiers dans les tests des contrôleurs.

La classe utilitaire **JdbcTemplate** (fournie par Spring-jdbc) peut être utilisée dans les tests métiers afin d'exécuter simplement des requêtes SQL en base de données pour valider le contenu.

La classe utilitaire **RestTemplate** (fournie par Spring-web) peut être utilisée dans les tests des contrôleurs pour réaliser les appels HTTP simplement.

**Selenium** est une API très riche. La classe MyDriver rassemble les opérations usuelles utiles aux tests (il est tout à fait possible de l'enrichir).
Les sélecteurs à utiliser dans les tests Selenium sont tous rassemblés dans la classe Selectors. Tous les sélecteurs doivent être rassemblés dans cette classe.
Cette classe contient une hiérarchie de sous-classes structurée ainsi : NomDeLaPage / TypeDelements / selecteurElement. Exemple : Index.Button.CONFIRMER_SUPPRESSION
Toute modification d'une page HTML doit entraîner l'enrichissement de la classe Selectors avec le sélecteur permettant de sélectionner les éléments ajoutés/modifiés/supprimés.

## Règles de conception
#### Persistance
Toute requête de recherche sans paramètre optionnel doit être codée dans un repository SpringData.
#### Service
La gestion des transactions et des erreurs se traite au niveau des services qui sont les points d'entrée et de sortie des transactions.
#### API
Les services ne sont exposés via l'API que s'ils sont utilisés par l'IHM.
La validation des paramètres d'entrée se fait dans le "contrôleur".
La transformation des entités en DTO se fait dans le "contrôleur" (dont c'est la responsabilité).
#### WEB - Pages
Les tableaux de données sont réalisés avec le plugin JQxGrid.
Dans un premier temps, les filtres, paginations et tris sont réalisés du coté client (par le framework).
Une fois l'usage courant du tableau défini, les bons filtres et tris (et donc aussi la pagination) doivent être implémentés coté serveur.
#### WEB - JS
Aucun script ne doit être présent dans la page HTML. A chaque page est associé un script. Les éléments communs à toutes les pages sont dans le script *common.js*.

## Règles de développement
Les règles ci-dessous doivent être appliquées par tout développeur souhaitant participer à ce projet (non négociable). **Par contre, il est toujours possible de discuter de la modification d'une règle.**

#### Langue
Les entités, les attributs et les méthodes sont nommés en français.
Les préfixes, suffixes et concepts inhérents au langage reste en anglais (get, set, is, iterator, ...)

#### Nommage
Les composants applicatifs sont suffixés en fonction de leur type : 

* "Application" pour les configurations SpringBoot,
* "Exception" pour les exceptions,
* "DTO" pour les structures de données de transport, 
* "Service" pour les services métiers, 
* "DAO" pour l'accès à la persistance, 
* "contrôleur" pour l'exposition de service en REST 
* et "Test" pour tous les tests

Les entités métier et les classes utilitaires ne sont pas suffixées mais doivent avoir des noms parlant (l'usage du nom d'un pattern est utile pour décrire l'usage/l'utilité du composant)
Les classes implémentant une interface sont suffixées par "IMPL"
Les noms de méthode commence par un verbe à l'infinitif

#### Mise en forme des sources
L'usage des Save Actions d'Eclipse est obligatoire pour conserver l'homogénéité du code (cf. [petit tour d'Eclipse]())

Le formatage des sources est celui par défaut dans Eclipse (version Néon) : 120 caractères par ligne (80 pour les commentaires) et indentation par tabulation.
Les membres sont triés systématiquement.
Les lignes ne contenant que des tabulations et/ou des espaces sont vidées.
Le mot clef "final" est placé sur toutes les variables affectées une seule fois et sur les paramètres.
Le mot clef "this" est utilisé systématiquement.
Les cast inutiles sont supprimés.

#### HQL
Les requêtes HQL simples peuvent être écrites sur une seule ligne. Les plus complexes sont formatées pour être plus lisible. L'usage des commentaires "//" en fin de ligne est autorisé pour forcer Eclipse à conserver à la ligne la chaîne de caractère à concaténer.

Toujours définir par défaut l'attribut READ_ONLY à *true* sur une méthode chargeant des entités persistantes. Seules les entités nécessitant d'être "managed" doivent l'être (cf. documentation hibernate sur le cycle de vie des objets dans une session)
Ne jamais créer un lien entre deux entités en instanciant une entité et ne définissant que son ID. Toujours utiliser *EntityManager.getReference*
La gestion des transactions est faite via les annotations Spring. Les exceptions du projet sont *runtime* et déclenchent donc le rollback. Seuls les composants de type "Service" et "DAO" peuvent être transactionnels (par les "contrôleur")

## Processus de développement
#### Gestion de configuration logicielle (GCL)
* Que comprend la GCL :
  * Les sources applicatives et de test sont dans un repository GIT ;
  * Le présent "guide du développeur" est lui aussi dans un repository GIT mais séparé (pas de gestion de version cohérente nécessaire) ;
  * Le bug tracker a utilisé est celui du repository GitHub ;
  * Les plans de test "métier" ne sont pas formalisés. Mais une partie d'entre eux sont automatisés avec Selenium.
* La gestion de version :
  * une seule version existe pour le moment (branche 'master').
  * si des POC sont réalisés, ils doivent l'être dans une branche dédiée
  * aucun TAG n'est posé pour le moment (c'est une chose à faire prochainement)

#### Commit 
Les messages de commit doivent être explicites. Toute nouvelle fonctionnalité doit être commitée avec un message commençant par "[FEATURE]" (afin que le commit soit correctement affiché dans la page *releaseNote* de l'application).

#### Industrialisation
##### Outils
La gestion des exigences est totalement informelle. Ce qui ne pose pas de problème pour ce projet car le délai entre l'expression du besoin et la mise en production est court et qu'aucune contractualisation n'existe (ni sur les moyens ni sur le périmètre métier).
GitHub héberge une partie des outils du projet : repository GIT & guide du développeur, ...
L'outil d'intégration continue utilisé est Jenkins (hébergé sur un serveur EC2).
La qualimétrie est réalisée avec Sonar (hébergée sur le site officiel en ligne).
Les relectures de code sont possibles avec GitLab mais ne sont pas utilisées actuellement (un seul développeur pour le moment)
##### Processus
Quitte à faire un petit projet sympa, autant utiliser les outils correctement. Donc le processus de développement (même s'il est minimal) est piloté par un pipeline (cf. [pipeline](/mesidees/13-pipeline/)).
Le pipeline est lui aussi en GCL : dans le *JenkinsFile* 
Le pipeline commence au commit du code et enchaîne les étapes suivantes : compilation, tests métiers, tests des contrôleurs, packaging, tests IHM, qualimétrie, promotion manuelle et mise en production


Sujets non traités ici (pour le moment) :

* conception
 * quels sont les outils de conception et/ou de modélisation ?
 * quels sont les objectifs des modèles (compréhension, communication, …) et quelle est leur durée de vie ?
 * règles de conception (programmation défensive, @designByContrat, modularité, ...)
 * jeu de données de test
* développement
 * liste des frameworks disponibles, leur usage et le pointeur sur leur documentation
 * qualimétrie obligatoire avec le site "online" (avec sa configuration imposée par la plateforme d'où les violations non corrigée qui ne le seront pas)
 * HQL propre
 * code en français
 * méthode = verbe à l'infinitif
 * largeur 120 (on a tous des résolutions d'écran suffisante)
 * ...
* industrialisation :
 * aucune installation de poste automatique car seul l'IDE est nécessaire et l'application est autoporteuse
 * IC = jenkins toujours à jour avec un pipelineAsCode (lien vers le blog sur le sujet)
 * pipeline = git/compile/TU/TI/TA/qualimétrie/promotion/production
 * à quand un second développeur et la mise en place des pullrequest et des revues de code ?
* roadmap :
 * thymeleaf ?
 * angular ?
 * tag à la promotion (fin du développement au fil de l'eau et passage en mode paquet de code)
 * ...
