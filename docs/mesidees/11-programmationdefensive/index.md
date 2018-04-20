---
title: Programmation défensive
weight: 111
---

Lorsqu'un développeur code une nouvelle méthode, il doit savoir s'il fait confiance au code qui fera appel à cette méthode. Ou pas.

* S'il a confiance, il déclarera les conditions d'appels à la méthode dans le contrat de sa méthode (dans la Javadoc de sa méthode, dans le WSDL, ou tout autre support d'échange avec le client) et ne vérifiera pas les valeurs des paramètres dès le début de sa méthode.
* S'il n'a pas confiance, les premières lignes de code de la méthode vérifieront que les valeurs des paramètres sont acceptables.
La seconde option est de la programmation défensive.

Une méthode peut valider les valeurs des paramètres d'entrée. Ce n'est pas une mauvaise chose en soit. Bien au contraire dans le cas de la validation des données lues dans un fichier envoyé par une application tierce. Même s'il existe un contrat définissant le format des données échangées, personne n'a envie d'intégrer des données de mauvaise qualité dans son système.
Mais une question se pose : "doit-on utiliser la programmation défensive partout ?". L'ajout de code vérifiant chaque paramètre de chaque méthode de chaque composant du système coûte du temps de développement (du code applicatif et des tests associés). De plus chaque ligne de code ajoute potentiellement un bug dans l'application. Et surtout la maintenabilité d'une application décroit avec l'augmentation du volume de code.

Mais il ne faut pas faire l'amalgame entre la programmation défensive et toutes les validations de données. Par exemple, la validation des données d'un formulaire saisies par un être humain est réalisée par une méthode dont c'est l'objectif. Dans ce cas, ce n'est pas de la programmation défensive. Mais, si le service métier appelé après la validation du formulaire vérifie lui aussi une donnée, là, ce sera de la programmation défensive.

Le choix entre programmation défensive et confiance absolue n'est pas du seul ressort du développeur. C'est un choix fait par l'équipe qui prend en compte le type de composant, le type de données, les contraintes techniques, ... Cela doit donner lieu à un chapitre dans le [guide du développeur](/mesidees/10-guidedudev/).

Références :

* http://etienne.charignon.free.fr/spip/spip.php?article68
* https://martinfowler.com/ieeeSoftware/failFast.pdf
* https://weblogs.asp.net/fredriknormen/defensive-programming-and-design-by-contract-on-a-routine-level
