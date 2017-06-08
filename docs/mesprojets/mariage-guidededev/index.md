---
title: Mariage - Guide du développeur
prev: /mesprojets/
next: /moncv/
weight: 201

---

## Exigences
Les exigences sont actuellement fournies par la principale utilisatrice. Aucune formalisation n'existe et un suivi minimaliste des tâches est réalisés dans le fichier "backlog.txt" placé à la racine de la branche "master".


## Tests
#### Environnement
Seul l'environnement de production, celui d'intégration et ceux de développement existent. Aucun environnement n'est dédié aux tests (techniques, métiers, performance ni sécurité).

Donc tous les tests doivent être :

* exécutés par le développeur sur son poste de développement, 
* codés avec JUnit ou TestNG,
* exécutés à chaque commit par l'intégration continue

#### Que tester
Les cas nominaux de tous les composants doivent être testés ainsi que les cas d'erreur principaux.

Aucune exigence n'impose une couverture de code particulière mais 80% est souhaitable pour tous les composants contenant de la logique (hors getter, setter, constructeur, code de [programmation défensive](/mesidees/11-programmationdefensive/), ...)

#### Comment tester
**Les composants métiers** (Service, Repostitory et Entité) doivent être testés avec des tests d'intégration. Il n'est pas concevable de tester un système entièrement basé sur la notion de transaction et de requêtes SQL (Spring, Hibernate et la bdd) sans une connexion à une base de données et sans activer les commits.

Si certains composants métiers présentent une grande complexité, alors un [test de granularité unitaire](/mesidees/03-testsdedeveloppement/) est tout à fait envisageable. Un tel composant n'existe pas pour le moment.

**Les contrôleurs REST** doivent être testés unitairement. Ces composants ne sont pas transactionnels et les services sont testés par ailleurs. Mais, pour bien vérifier le paramétrage et le code, il est nécessaire de réaliser le test des contrôleurs en démarrant un serveur WEB (l'adjectif "unitaire" utilisé plus tôt est donc discutable - cf. [test de granularité unitaire](/mesidees/03-testsdedeveloppement/)).

**Les écrans** de l'application sont testés de bout en bout. Selenium est utilisé pour piloter le navigateur et un jeu de données est utilisé pour initialiser le test.

ATTENTION : les tests Selenium ne sont donc pas tous indépendants les uns des autres. Ce n'est pas une bonne pratique mais faire autrement avec un test de bout en bout est trop coûteux.

#### Avec quels outils
JUnit et TestNG sont tous deux des exécuteurs de tests. JUnit est utilisé dans les tests des composants métiers. TestNG, étant plus pratique à utiliser avec Mockito, est utilisé pour l'exécution des tests des contrôleurs.

Mockito est donc utilisé pour bouchonner les services métiers dans les tests des contrôleurs.

La classe utilitaire JdbcTemplate (fournie par Spring-jdbc) peut être utilisée dans les tests métiers afin d'exécuter simplement des requêtes SQL en base de données pour valider le contenu.

La classe utilitaire RestTemplate (fournie par Spring-web) peut être utilisée dans les tests des contrôleurs pour réaliser les appels HTTP simplement.

Selenium est une API très riche. La classe MyDriver rassemble les opérations usuelles utiles aux tests (il est tout à fait possible de l'enrichir).
Les sélecteurs à utiliser dans les tests Selenium sont tous rassemblés dans la classe Selectors. Tous les sélecteurs doivent être rassemblés dans cette classe.
Cette classe contient une hiérarchie de sous-classes structurée ainsi : NomDeLaPage / TypeDelements / selecteurElement. Exemple : Index.Button.CONFIRMER_SUPPRESSION
Toute modification d'une page HTML doit entraîner l'enrichissement de la classe Selectors avec le sélecteur permettant de sélectionner les éléments ajoutés/modifiés/supprimés.

## Gestion de configuration logicielle
#### Source
Les sources sont toutes rassemblées dans GitHub. Le développement se fait directement dans la branche "master". Les essais/recherches/prototypes éventuels doivent être codés dans d'autres branches (*/!\ le nom des branches est pris en compte par l'intégration continue pour la promotion et le déploiement en production - cf. Jenkinsfile*).

Les messages de commit doivent être explicites. Toute nouvelle fonctionnalité doit être commitée avec un message commençant par "[FEATURE]" (afin que le commit soit correctement affiché dans la page *releaseNote* de l'application).



TO CONTINUE



* conception
 * résumé de l'architecture technique
 * quels sont les impacts de l’AT sur la conception (BI, ERP ou détails trop fins en xNet) ?
 * quel est le niveau de détails attendu en fin de conception (peut-être besoin d’être plus précis que le niveau contractuel en fonction du contexte) ?
 * quels sont les outils de conception et/ou de modélisation ?
 * quels sont les objectifs des modèles (compréhension, communication, …) et quelle est leur durée de vie ?
 * règles de conception (programmation défensive, @designByContrat, modularité, ...)
 * jeu de données de test

* développement
 * règles de nommage
 * liste des frameworks disponibles, leur usage et le pointeur sur leur documentation

* industrialisation
 * éléments composant la Gestion de Configuration Logicielle (Git, SVN, HP ALM, Archiva, Jira, ...)
 * règles d'usage de la GCL (flow GIT/SVN, snapshots dans le repository de binaires, ...)
 * règles d'usage de l'intégration continue
 * organisation des équipes en amont et en aval des développements (Continuous Delivery)

* environnements
 * installation du poste de développement
 * liste et paramètres des environnements de test
 * liste des environnements du client
