---
title: Eclipse
prev: /mesidees/11-programmationdefensive/
next: /mesidees/13-pipeline/

weight: 112
---

* [Introduction](#introduction)
* [Un outil mal/sous utilisé](#un-outil-mal-sous-utilisé)
* [Comment apprendre à utiliser pleinement Eclipse ?](#comment-apprendre-à-utiliser-pleinement-eclipse)
* [Les raccourcis à connaître](#les-raccourcis-à-connaître)
* [Les fonctionnalités natives indispensables](#les-fonctionnalités-natives-indispensables)
* [Les plugins très utiles en fonction des projets](#les-plugins-très-utiles-en-fonction-des-projets)


#### Introduction

Eclipse est un IDE, un environnement de développement intégré. A ce titre, il propose un éditeur de texte et des centaines de fonctionnalités autour. Basé sur un système de plugin, il est très extensible et très personnalisable.

Eclipse n'est pas le seul IDE disponible sur le marché. NetBeans, IntelliJ sont d'autres solutions. Mais je ne fournirais pas de comparatif car je n'ai que très peu utilisé NetBeans et jamais utilisé IntelliJ. C'est une bêtise (je le sais) car il est toujours intéressant de découvrir un nouvel outil pour voir s'il n'est pas meilleur sur certains points.

#### Un outil mal/sous utilisé

Mais quel que soit l'outil, une fois choisi, encore faut-il savoir s'en servir !

Si vous utilisez un IDE à la place de MS WordPad, il doit y avoir une raison ! 

* Alors cessez de taper chaque lettre de chaque mot de votre code et **utilisez l'auto-complétion !** 
* Plus de mise en forme du code avec des séries frénétiques de coups sur la barre d'espace et **utilisez le formateur automatique !**
* Ne recherchez pas un fichier dans l'arborescence mais **utilisez le formulaire de recherche !**
* Ne compter pas le nombre de ligne depuis le début du fichier mais **utiliser le GoToLine** (cf. raccourcis plus bas) !

*(Ces illustrations ne sont pas le fruit de mon imagination mais viennent de mes observations durant les formations Java que j'ai dispensées en 2016 et 2017 auprès de jeunes développeurs)*

#### Comment apprendre à utiliser pleinement Eclipse ? 

A plusieurs occasions, cette question m'a été posée "Comment apprendre à utiliser pleinement Eclipse ?".

Ma première réponse était de prendre 30 à 45 minutes pour parcourir les nombreux formulaires de paramétrage d'Eclipse pour découvrir toutes les fonctions que l'outil propose :
![eclipse-mousefeed](/images/eclipse-preferences.png)

Oui, en effet, il existe un très grand nombre de formulaires de paramétrage. Non, vous n'utiliserez certainement jamais toutes les fonctionnalités de l'outil mais il faut en connaître un minimum.

Dernièrement (en mai 2017), un collègue m'a fait découvrir MouseFeed. Ce plugin d'Eclipse, au clic sur un bouton ou un menu contextuel, affiche quelques instants le raccourci clavier 
![eclipse-mousefeed](/images/eclipse-mousefeed.png)

#### Les raccourcis à connaître

Le minimum du minimum :

* Dans Eclipse en général :
  * **Shift + Ctrl + T** pour la recherche d'une classe (d'un Type)
  * **Shift + Ctrl + R** pour la recherche d'un fichier quel que soit son extension (d'une Ressource)
* Dans un éditeur de code Java
  * **F3** pour se déplacer à la déclaration d'une variable, d'un paramètre, d'une méthode ou d'une classe
  * **Ctrl + O** (la lettre) pour afficher une popup listant les membres et méthodes et permettant de se déplacer rapidement 
  * **Ctrl + L** pour se déplacer à un numéro de ligne dans le fichier (GoToLine)
  * **Shift + Ctrl + S** pour sauvegarder tous les fichiers ouverts et en cours de modification
* Les raccourcis à oublier grâce aux SaveActions (cf. plus bas) :
  * Shift + Ctrl + F pour le formatage des sources
  * Shift + Ctrl + F pour la réorganisation des imports
* Les raccourcis avancés :
  * **Shift + Ctrl + X** pour exécuter la classe en cours de modification (raccourci à faire suivre du type d'exécution T pour JUnit, N pour TestNG, ...)
  * **F4** pour afficher la hiérarchie d'une classe
  * **Shift + Ctrl + G** pour rechercher les utilisations d'une méthode

#### Les fonctionnalités natives indispensables

La première à configurer : le formateur de code car chaque client/projet a ses contraintes. Et le formatage ne doit pas être fait à la main !!!!!!!

La plus belle des fonctionnalités natives d'Eclipse se nomme "Save Actions".
Son but : à chaque sauvegarde d'une classe Java, il exécute un certain nombre d'action telle que :

* le formatage des sources
* la réorganisation des imports
* le tri des membres et méthodes (ordre configurable)
* la suppression des espaces/tabulations dans les lignes vides
* l'ajout du mot clef *final* systématique (configurable)
* l'ajout du mot clef *this* systématique (configurable)

#### Les plugins très utiles en fonction des projets

Le plugin **JBoss Tools** contient les **Hibernate Tools**.
Ce plugin permet de configurer un éditeur de requête HQL lié à une base de données et une configuration JPA (ou Hibernate) et permet l'exécution et le débogage de requêtes.
(cf. [billet décrivant l'usage de ce plugin](/mesidees/04-consolehibernate/))
