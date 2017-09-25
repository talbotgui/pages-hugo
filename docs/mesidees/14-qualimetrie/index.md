---
title: Qualimétrie
prev: /mesidees/13-pipeline/
next: /mesidees/15-js/

weight: 114
---

La Qualimétrie est la mesure de la qualité d'un projet. 

## Le problème qui se pose avant de parler de qualimétrie.

Or, qualité veut tout et rien dire. Donc, ici, je vais me limiter à la qualité du code.

Mais pour évaluer le code d'un projet, encore faut-il qu'il existe des règles de conception/développement sur le projet. Sans règle, il ne peut y avoir de qualité.

**Et oubliez l'expression *"les standards de qualité"* !!!** Vous en voulez des standards ? On prend les règles de développement de SUN ? Ou celles d'Eclipse ? Ou les règles de nommage de la convention JavaBean ? Ou celles du client (s'il en a défini et qu'elles ne sont pas obsolètes) ?

Donc, par pitié, que chaque projet définisse ses règles ! En s'appuyant sur des éléments existants bien entendu et sans redécouvrir la roue !

**A partir de là, on peut commencer à mesurer la qualité du code.**

## Pourquoi faire
*"La qualité ne sert à rien."* Si vous le pensez, quittez ce site !

Vous continuez à lire. Tant mieux. Désolé de paraître dur mais il n'est pas acceptable d'ignorer la qualité d'un projet.
Elle peut ne pas être prioritaire. Mais ça se paiera tôt ou tard.

Pourquoi ? **Car un code moche est un code compliqué à faire évoluer !**

Les développeurs prendront des raccourcis et feront du code moche car le code est déjà moche et que tout le monde le sait et s'en moque. Et du moche dans du moche, au milieu du moche, ... donnera un code incompréhensible, incohérent et **très difficile à maintenir.**

Ce jour-là, apparaîtront des bugs en pagaille et les corrections prendront de plus en plus de temps.

## Que comprend la qualimétrie de code

Les règles de conception/développement portent sur beaucoup d'aspects du code. Donc la qualimétrie aussi :

* format du code : 
  * longueur des lignes de code,
  * longueur des lignes de commentaire,
  * nombre de lignes maximal d'une méthode,
  * nombre de lignes maximal d'une classe,
  * tabulation en espace ou non,
  * indentation
  * ...
* lisibilité et compréhensibilité du code :
  * longueur des noms de variables/paramètres/membres/méthodes/classes/interfaces/package,
  * mauvais usages des noms réservés (i, j, T, ...)
  * ...
* complexité des algorithmes :
  * nombre maximal de boucles imbriquées,
  * méthodes récursives, 
  * mauvais usages de mots clefs (*return*, *break*, *continue*, ...)
  * ...
* couverture de code
  * pourcentage de lignes de code testées
  * pourcentage de combinaisons de conditions (if, while, ...) testées
* sécurité
* duplication
* ...

## Quels outils
L'outil le plus connu est SonarQube.

Ce n'est pas à proprement parlé un outil de qualimétrie. C'est un portail de restitution et agrégation des violations détectées par d'autres outils comme Checkstyle, FindBugs, PMD, CPD, Clover, Cobertura, ... De plus, SonarQube permet d'administrer le paramétrage de ces analyseurs et d'exécuter les analyses.

C'est donc un outil très complet et indispensable !

SonarQube propose une [plateforme en ligne] (https://sonarcloud.io), gratuite et publique pour les projets Open Source.

## Comment démarrer

Que votre projet démarre tout juste, ou que vous ayez déjà des milliers de lignes de code, il est toujours temps de commencer à mesurer la qualité.

Le premier objectif de la mesure est de faire un point. Est-ce une catastrophe ? Ou pas ? **Attention, il faut que le paramétrage des outils d'analyse corresponde à vos règles !!** Si vous prenez le paramétrage par défaut de SUN sur votre projet sans l'adapter à votre contexte, vous obtiendrez des milliers de violations.

Vous avez votre première mesure ? Très bien ! Première chose à faire, que la dette soit petite ou grande, **ne pas la dégrader !** Surveiller les violations et, à chaque nouveau problème détecté, trouvez le développeur, apprenez/rappelez lui les règles du projet et faites-lui corriger sa coquille.

Une fois toute hémorragie contenue, on peut s'attaquer à réduire les violations. Il existe plusieurs stratégies et deux d'entre elles me semblent pertinentes à mener en parallèle :

* trouver, quel que soit leur sévérité, les violations les plus nombreuses mais corrigibles en quelques minutes (avec un outil ou un rechercher/remplacer d'expression régulière)
 * avantages : diminuer le nombre de violations rapidement et simplement
 * soucis : les développeurs risquent d'en recréer car la correction est un bon moyen d'apprendre
* faire corriger, chaque semaine, par chaque développeur, un petit lot de violations parmi les plus importantes 
 * avantages : les violations diminueront régulièrement, les développeurs apprendront durant les corrections, le temps passé à améliorer le code sera réparti sur plusieurs personnes et plusieurs semaines
 * soucis : la qualité augmentera lentement (à condition qu'aucune hémorragie ne subsiste), une personne aura la charge d'analyser les rapports SonarQube pour lister les violations à traiter chaque semaine


## Les travers et excès

La mesure de la qualité doit être une aide. Pas un frein !

* Il faut analyser et extraire uniquement les problèmes violant les règles du projet (et uniquement elles) ;
* SonarQube n'est pas un outil de surveillance de la production des développeurs : la découverte d'une violation ne doit pas donner lieu à un lynchage public ;
* L'outil est au service de l'équipe et non d'un râleur prenant plaisir à critiquer le travail des autres.

## Mieux que surveiller, prévenir !

SonarQube analyse le code présent sur le serveur hébergeant les sources.

Mais, le mieux serait qu'un outil prévienne le développeur dès qu'il code une bêtise !

La réponse : SonarLint ! Cet outil (sous forme de plugin dans les IDE) se connecte à SonarQube, télécharge les règles de validation et vérifie le code directement dans l'environnement du développeur.

Avec cet outil, si un développeur crée encore des violations, c'est qu'il se moque de son équipe. Là, très chères collègues, je vous laisse lui expliquer votre point de vue entre gens courtois et polis (dans la mesure du possible ;) ).
