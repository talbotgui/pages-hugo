# Description du contenu #
Ce repository contient les sources utilisées par Hugo pour généré le site talbotgui.github.com (https://gohugo.io/overview/introduction).

# Afficher uniquement le site en local #
Il suffit de démarrer le script startServer.cmd sur un Windows ou startServer.sh sur un Unix.

# Générer l'index de recherche #
Pour générer l'index permettant la recherche, il est nécessaire d'avoir installé NPM.
Pour cela, télécharger NodeJS depuis le site http://blog.teamtreehouse.com/install-node-js-npm-windows
Puis ouvrir une fenêtre de commande dans le répertoire contenant le fichier package.json
Pour installer l'outil d'indexation, exécuter la commande "npm install"

# Pour vérifier qu'aucun lien n'est cassé #
L'outil "broken-link-checker" permet de vérifier que tous les liens du site fonctionnent.
Pour l'installer, exécuter la commande "npm install broken-link-checker -g".
Pour l'exécuter sur le site localement, exécuter la commande "bin\checkBrokenLinks.cmd"
