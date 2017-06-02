---
title: Dette technique
prev: /mesidees/
next: /mesidees/02-pratiques/
weight: 101

---

#### Définitions sur le net
  
* Ward Cunningham (1992) : if you develop a program for a long period of time by only adding features and never reorganizing it to reflect your understanding of those features, then eventually that program simply does not contain any understanding and all efforts to work on it take longer and longer.
* James Shore : the cumulative total of less-than-perfect design and implementation
* Tom Poppendieck : everything that makes your code harder to change

#### Ma définition
La dette technique 

* est constituée des imperfections du design et du code
* réduit la productivité de l'équipe
* réduit la maintenabilité
* augmente le nombre d'erreurs (bugs dans les nouvelles fonctionnalités et régressions dans le code existant)
* se mesure par l'effort qu'il faut pour la résorber

Ces imperfections/violations se séparent en 3 groupes :

* les imperfections **identifiées et délibérées** : elles sont les preuves de mauvais choix fait en toute connaissance de cause. Ces choix doivent impérativement donner lieu à un plan d'action pour résoudre le problème. Exemple : pas de mesure de la couverture de code ni de vérification des exigences qui y sont liées dans un premier temps à cause des délais. Mais on y reviendra juste après la livraison.
* les imperfections **involontaires et relevées par un outil** : elles sont les preuves d'une incompétence ou d'une méconnaissance. Exemple : les violations relevées par Sonar sur code commité. Ces violations doivent être traitées au plus vite par leur auteur. Ainsi il apprendra de ses erreurs et ne recommencera plus.
* les imperfections **involontaires et non identifiées** : elles sont souvent identifiées durant la résolution d'un bug. Le problème est général à tout le code mais c'est un code en particulier qui a fait émerger un problème. Il faut alors ajouter une règle de développement sur le projet (documenter la règle et l'intégrer dans les outils de vérification). Exemple : un caractère bizarre dans une page WEB va permettre de se rendre compte qu'une partie des fichiers "source" n'est pas en UTF8.
	
La dette se traite en

* définissant au plus tôt des règles de conception, de développement et de test
* mettant en place des outils (sur le poste du développeur et dans l'intégration continue)
* sensibilisant/formant les développeurs pour qu'ils génèrent moins de dette
* attribuant une priorité à chaque imperfection existante (ou type d'imperfections)
* organisant des actions de refactoring pour réduire le nombre d'imperfections
* amendant et enrichissant les règles tout au long du projet

#### Sources

* [Technical debt - Ward Cunningham] (http://www.c2.com/cgi/wiki?WardExplainsDebtMetaphor)
* [Technical debt - Ward Cunningham] (https://www.youtube.com/watch?v=pqeJFYwnkjE)
* [Technical debt - Martin Fowler] (http://martinfowler.com/bliki/TechnicalDebt.html)
* [Measuring and managing technical debt - CAST Software] (http://www.omg.org/news/meetings/tc/tx-14/special-events/cisq-presentations/CISQ-Seminar-2014-6-17-BILL-CURTIS-Measuring-and-Managing-Technical-Debt.pdf)
