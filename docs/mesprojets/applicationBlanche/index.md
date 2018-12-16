---
title: Application Blanche
weight: 203
---

Cette page est un court guide du développeur ([ma définition du guide du développeur](/mesidees/10-guidedudev/)) pour un projet assez particulier : une [Application Blanche] (https://github.com/talbotgui/appliBlanche).

L'objectif de l'application blanche est de fournir un exemple d'application (régulièrement utilisé quand je donne des formations) mais aussi une structure minimale pour créer une nouvelle application. A ce titre, l'application doit être particulièrement bien codée (dans le fond comme dans la forme) mais aussi bien documentée (il y a du boulot de ce coté-là).

**Sommaire** :

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
* [Règles de développement] (#règles-de-développement)
  * [Langue] (#langue)
  * [Nommage] (#nommage)
  * [Mise en forme des sources] (#mise-en-forme-des-sources)
  * [Spring Data] (#spring-data)
  * [HQL] (#hql)
  * [Angular] (#angular)
  * [VueJS] (#vuejs)
* [Processus de développement] (#processus-de-développement)
  * [Gestion de configuration logicielle] (#gestion-de-configuration-logicielle-gcl)
  * [Commit] (#commit)
  * [Industrialisation] (#industrialisation)
* [Installation des environnements] (#Installation-des-environnements)
* [FAQ] {#faq}

## Exigences
Le fichier "todoList.txt" présent à la racine de la branche *master* contient une backlog.

#### métiers
L'application n'a pas d'objectif métier. 

#### techniques
Les sources évoluent au grès des besoins techniques que je veux prototyper et/ou des nouveaux usages/frameworks que je découvre.

Mais, l'application doit tout de même répondre aux exigences suivantes :

* facilement accessible sur tout support : PC, tablette, téléphone ;
* évolutive : chaque nouveau besoin doit être facilement réalisé (toujours resté sur des solutions simples et efficaces) ;
* fiable (point primordiale) : l'application ne doit pas subir de régressions intempestives portant atteinte à son utilisabilité ou à la fiabilité de ses données.

## Environnement
Ce projet n'était pas déployé où que ce soit jusqu'en septembre 2018. Avant cette date, seule la PIC démarre l'application à travers les tests (unitaires, d'intégration et d'application).
Depuis cette date, l'application est déployée sur le serveur Unix et sera bientôt déployée sur le cloud de Google (GCP).

## Architecture technique

Cette application se compose :

* d'un backend coupé en quatre modules :
 * un projet contenant les classes réutilisables pour les projets "métiers"
 * un projet contenant les classes réutilisables pour les projets "REST"
 * un projet de services métiers et de persistence
 * un projet de composants REST sécurisés (avec JWT)
* d'une application WEB développée avec Angular
* de la même application WEB (même écrans et fonctionnalités) développée avec VueJS
* de batch (simple en SQL et une autre avec JPA)

Les applications WEB sont simples (voire simpliste) et mettent en oeuvre des composants différents pour faire des tests :

* un formulaire Material
* l'internationnalisation avec des libellés venant d'une API
* une application PWA
* table Material
* ...

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
La validation des paramètres d'entrée se fait dans le "contrôleur" et dans l'écran.

## Règles de développement
Les règles ci-dessous doivent être appliquées par tout développeur souhaitant participer à ce projet (non négociable). **Par contre, il est toujours possible de discuter de la modification d'une règle.**

#### Langue
Les entités, les attributs et les méthodes sont nommés en français.
Les commentaire sont en français.
Les préfixes, suffixes et concepts inhérents au langage reste en anglais (get, set, is, iterator, ...)

#### Nommage
Les composants applicatifs sont suffixés en fonction de leur type : 

* "Application" pour les configurations SpringBoot,
* "Exception" pour les exceptions,
* "DTO" pour les structures de données de transport, 
* "Service" pour les services métiers, 
* "DAO" pour l'accès à la persistance, 
* "REST" pour l'exposition de service en REST 
* et "Test" pour tous les tests

Les entités métier et les classes utilitaires ne sont pas suffixées mais doivent avoir des noms parlant (l'usage du nom d'un pattern est utile pour décrire l'usage/l'utilité du composant)
Les classes implémentant une interface sont suffixées par "IMPL"
Les noms de méthode commence par un verbe à l'infinitif

#### Mise en forme des sources
L'usage des Save Actions d'Eclipse est obligatoire pour conserver l'homogénéité du code (cf. [petit tour d'Eclipse]())

Le formatage des sources est celui par défaut dans Eclipse (version Oxygen) : 150 caractères par ligne (150 pour les commentaires) et indentation par tabulation.
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

#### Spring Data
Les requêtes sont, pour la quasi totalité d'entre elles, positionnées dans un Repository Spring Data. Elles sont donc dans les annotations d'une interface et non dans une classe.
L'usage de méthode *default* est interdit sur mon projet. Si je dois coder un algorithme, il doit être dans le service. Si la requête est complèxe (multi-critères optionnels par exemple), il faut créer une classe à part.

Astuces et penses-bêtes :

* ne jamais mettre de clause *order by* dans une requête si la requête est paginée (usage d'un paramètre de type org.springframework.data.domain.Pageable)
* toujours définir la requête *countQuery* d'une requête paginée si la requête contient un *join fetch*

#### REST
Les méthodes de contrôleur REST retournant uniquement une chaine de caractères doivent retourner la dite chaine entre *double quote*.

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
GitHub héberge une partie des outils du projet : repository GIT & guide du développeur, ...
L'outil d'intégration continue utilisé est Jenkins (hébergé sur un serveur EC2).
La qualimétrie est réalisée avec Sonar (hébergée sur le site officiel en ligne).
Les relectures de code sont possibles avec GitHub mais ne sont pas utilisées actuellement (un seul développeur pour le moment)
##### Processus
Quitte à faire un petit projet sympa, autant utiliser les outils correctement. Donc le processus de développement (même s'il est minimal) est piloté par un pipeline (cf. [pipeline](/mesidees/13-pipeline/)).
Le pipeline est lui aussi en GCL : dans le *JenkinsFile* 
Le pipeline commence au commit du code et enchaîne les étapes suivantes : compilation, tests métiers, tests des contrôleurs, packaging, tests IHM, qualimétrie, promotion manuelle et mise en production

## Installation des environnements

### Sur le serveur unix
Le déploiement sur un serveur Unix demande la création 
* d'un répertoire hébergeant l'application WEB dans un sous répertoire de /var/www/html. 
 * Attention à bien changer le root dans la configuration du build pour prendre en compte le nom du sous répertoire dans /var/www/html/.
 * De plus, il faut donner les droits à l'utilisateur Jenkins d'écrire dans ce répertoire.
 * Enfin, il faut modifier le fichier de configuration Apache pour permettre le chargement du WORKER : ```Header set Content-Security-Policy "worker-src https://*.guillaumetalbot.com;"```
* d'un répertoire hébergeant le Backend
 * Le répertoire doit contenir des scripts de démarrage et d'arrêt (cf. sources de l'application)
 * Le répertoire doit aussi contenir les fichiers de configuration propres à l'environnement (log4j2.xml et application.properties)
 * De plus, il faut donner les droits à l'utilisateur Jenkins d'écrire dans ce répertoire pour le JAR, les logs et la bases de données (attention aux fichiers créés par un autre compte que Jenkins !).
 * Il ne faut pas oublier d'ajouter les redirections dans la configuration Apache !!

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
 * ...
* industrialisation :
 * aucune installation de poste automatique car seul l'IDE est nécessaire et l'application est autoporteuse
 * IC = jenkins toujours à jour avec un pipelineAsCode (lien vers le blog sur le sujet)
 * pipeline = git/compile/TU/TI/TA/qualimétrie/promotion/production
 * à quand un second développeur et la mise en place des pullrequest et des revues de code ?
* roadmap :
 * tag à la promotion (fin du développement au fil de l'eau et passage en mode paquet de code)
 * ...

## FAQ

* Erreur de sérialisation d'un attribut @OneToMany dans Jackson à cause d'une LazyInitializationException
 * Utiliser la méthode MutableUtil.getMutable dans le getter