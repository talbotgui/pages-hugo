---
title: Object Mother
prev: /mesidees/08-creationavechugo/
next: /mesidees/10-guidedudev/

weight: 109
---

## Le pattern tel qu'il est défini aujourd'hui

Ce pattern est dédié à la création d'objets de données utilisés dans des tests codés.

Ce pattern se base sur le pattern Factory. Il propose la création d'une classe dont la responsabilité est de créer des objets.
La particularité de ce pattern est que les données créées sont dédiés aux tests.

Extraire le code de création de ces objets améliore la lisibilité des tests (à condition que le nom des méthodes de l'ObjectMother soit explicite).
De plus, le fait d'utiliser systématiquement un jeu de données particulier permet de créer des données "familières". Par exemple : Jean est un nouvel utilisateur, Arnaud est un utilisateur avec des droits d'administration, ... 
A l'usage, ces persona (ou leurs pendants quel que soit le type d'objets manipulés : facture, usine, paie, compteur, ...) finissent pas être connus de tous les développeurs. Ceci facilite la conception, le développement et la mise au point des tests.

Sources :

* [ObjectMother sur le site C2.COM] (http://wiki.c2.com/?ObjectMother)
* [ObjectMother sur le blog de Martin Fowler] (https://martinfowler.com/bliki/ObjectMother.html)
* [ObjectMother vs TestDataBuilder] (http://blog.codeleak.pl/2014/06/test-data-builders-and-object-mother.html)

Exemples :

* [exemple explicatif] (https://github.com/iluwatar/java-design-patterns/tree/master/object-mother)
* [cas concret dans une petite application] (https://github.com/scribejava/scribejava/blob/master/scribejava-core/src/test/java/com/github/scribejava/core/ObjectMother.java)
* [cas concret dans une petite application] (https://github.com/talbotgui/mariage/blob/master/mariageMetier/src/test/java/com/github/talbotgui/mariage/metier/service/ObjectMother.java)

## Les risques associés à ce pattern
Ce pattern ne doit pas dérivé en anti-pattern "God Class". Un ObjectMother ne doit pas devenir un composant trop gros ni dépendant de trop d'autres composants. 
Pour cela, il suffit de créer un ObjectMother par groupe de données (par exemple : SecuriteObjectMother, FacturationObjectMother, ...).
Et pour limiter le volume de code d'un ObjectMother, il suffit de respecter quelques règles :

* pour créer un nouveau jeu de donnée, il faut créer une nouvelle méthode
* chaque méthode créant des données doit utiliser une méthode déjà existante créant un jeu de données plus simple et doit enrichir ce jeu de données
* seul l'ajout ou la modification d'un attribut obligatoire peut donner lieu à la modification d'une méthode existante. Mais attention, cela nécessite de revérifier tous les tests utilisant la méthode modifiée et les méthodes dérivées)
