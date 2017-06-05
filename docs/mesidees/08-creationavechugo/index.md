---
title: Création d'un site avec Hugo
prev: /mesidees/07-hugoetgithubpages/
next: /mesidees/09-objectmother/
weight: 108

---
## La structure

Le plus simple est toujours de partir d'une base de travail simple.
Pour cela, sont disponibles plusieurs base :

* [SIMPLE] (https://github.com/talbotgui/creation-hugo/tree/simple) : un site Hugo de 2 pages avec le thème [LEARN] (https://)
* [INDUS] (https://github.com/talbotgui/creation-hugo/tree/indus) : le même site SIMPLE mais industrialisé avec NPM pour démarrer le site sur Windows, Unix ou simplement générer le site
* [SEARCH] (https://github.com/talbotgui/creation-hugo/tree/search) : le même site INDUS avec la fonction de recherche basée sur LUNR
* [JENKINS] (https://github.com/talbotgui/pages-hugo/tree/master) : mon site est basé sur SEARCH mais automatiquement publié sur les GitHub:pages

## Les astuces à connaître

#### Hugo en 2 mots

L'idée d'Hugo est de générer un site WEB statique avec une jolie mise en forme sans se prendre la tête avec la mise en forme.

Donc le contenu du site est codé en Markdown et Hugo génère les pages WEB.

L'avantage d'un site statique est qu'il est extrêmement performant. Cette solution est utilisée en production pour générer des pages WEB dont la fréquence de modification des contenus est faible (1 à 2 fois par jour).

#### Manipulation
Le site se démarre en double-cliquant sur "/bin/startServer.cmd". Le site est alors disponible sur l'adresse "http://localhost:1313".

Toute modification d'une page existante déclenche automatiquement son rafraîchissement dans le navigateur. Mais l'ajout d'une nouvelle page n'est pas pris en compte à chaud. Il faut couper le serveur et le redémarrer.

#### Contenu et mise en forme
Les premières lignes de chaque page (les fichiers ".MD" du répertoire "docs") sont des propriétés :

* title : le titre de la page utilisé dans l'onglet affichant la page mais aussi dans le menu latéral de la page
* prev : [optionnel] le chemin vers la page précédente qui doit toujours commencer et se finir par un "/". Il est possible de naviguer par le menu mais aussi par les flèches présentes à gauche et à droite des articles ou encore avec les flèches gauche et droite du clavier.
* next : [optionnel] le chemin vers la page suivante
* weight : [optionnel] l'importance de la page et donc son ordre d'apparition dans le menu
* chapter : [optionnel] 'true' si la page est un chapitre. Le contenu est alors centré par défaut.

#### Outil utile
Un site avec Hugo utilisant "prev" et "next" contient donc beaucoup de lien. Il faut les vérifier. Pour le faire automatiquement et rapidement, il existe des outils comme "Broken Link Checker"
 outil de check des liens
 ...
