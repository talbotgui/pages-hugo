---
title: Tests de développement
prev: /mesidees/02-pratiques/
next: /mesidees/04-consolehibernate/
weight: 103

---


J'en ai ras le bol de ce débat stérile sur les tests unitaires. Chacun a sa petite définition et, en fonction du contexte, utilise mal l'adjectif unitaire.

D'un côté, les tests sont unitaires quand le SUT (System Under Test) se limite à une classe voire une méthode. L'avantage de ces tests est qu'ils sont assez simples à rédiger car le code à tester se limite à celui de la classe (inutile de connaître le comportement de tous les autres composants du système qui peuvent être mis en jeu). Le problème est qu'il faut justement arriver à isoler le SUT et donc utiliser des techniques de bouchonnage (encore un framework que les collaborateurs du projet doivent apprendre à maîtriser).

De l'autre côté, les tests sont unitaires du moment qu'ils participent à la construction de l'application durant les travaux du développeur. Et ce quel que soit la granularité du SUT, que les tests soient unitaires (selon la première définition), d'intégration ou applicatif (de bout en bout avec tous les composants et systèmes externes).

Et si, pour se simplifier la vie, on parlait de tests de développement utilisant des TU, TI et TA ?

Et là, on me pose la question “Et les tests de non-régression ?”. Ma réponse est simple “Un test de non-régression a été un test de développement mais le développement est fini et le test est resté.”
