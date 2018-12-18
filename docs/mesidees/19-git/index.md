---
title: Git
weight: 119
---


#### Les basics :

Pour cloner un dépot : ```git clone https://xxxxxxxxx```

Pour ajouter une modification : ```git add rep1/monFichier.xxx```

Pour ajouter toutes les modifications : ```git add -A```

Pour annuler les ADD : ```git reset```

Pour annuler le ADD d'un fichier : ```git reset rep1/monFichier.xxx```

Pour annuler les modifications d'un fichier : ```git checkout rep1/monFichier.xxx```

Pour commiter : ```git commit -m "mon message"```

Pour commiter les fichiers modifiés (pas les ajouts) sans faire de ADD : ```git commit -am "mon message"```

Pour pousser ces commits sur le repository : ```git push ```

Pour récupérer toutes les modifications réalisées depuis le dernier PULL  : ```git pull```ou  ```git pull --rebase```si des commits ont été réalisés depuis le dernier PULL

En cas de problème durant un rebase, la commande ```git rebase --abort```annule tout.

#### Les utilitaires :

Pour voir le statut du dépot local : ```git status```

Pour voir les modifications d'un fichier : ```git diff rep1/monFichier.xxx```

Pour voir toutes les modifications : ```git diff```

Pour obtenir les logs : ```git log```ou ```git log --pretty=oneline```

Pour afficher les informations du repository distant : ```git remote show origin ```

Pour sauvegarder dans un espace temporaire des modifications (sans les commiter) : ```git stash```

Pour lister les sauvegardes : ```git stash list```

Pour récupérer la sauvegarde la plus récente : ```git stash pop```

Pour annuler tous les commits locaux : ```git reset HEAD^```

#### La manipulation d'historique :

Pour modifier l'historique sur les 5 derniers commits (à ne faire que si aucun push n'a été fait depuis) : ```git rebase --interactive HEAD~5 ```

Dans l'éditeur qui s'ouvre, remplacer les *pick* par :

* *r* pour *reword* afin de modifier le message du commit (le nouveau message est à saisir après avoir sauvegarder ce premier éditeur de texte)
* *s* pour *squash* permet de fusionner le commit avec celui qui le précède dans le temps (et fusionne les commentaires des deux commits)
* *f* pour *fixup* qui fusionne comme le squash mais qui supprime le commentaire

Pour pousser sur le repository distant un rebase qui modifie l'historique déjà poussé /!\ attention quand on travaille en équipe /!\ : ```git push --force```

#### La manipulation de branches :

Pour lister les branches locales : ```git branch```

Pour passer d'une branche à l'autre : ```git checkout maBranche```

Pour créer et passer à une autre branche : ```git checkout -b maBranche```

Pour supprimer une branche : ```git branch -d maBranche```

Pour afficher les différences entre deux branches : ```git dif maBranche...lAutreBranche```

Pour fusionner simplement une branche vers la branche courante : ```git merge monAutreBranche```

Pour pousser dans le master les modifications d'une autre branche en un seul commit : 
```
git checkout master
git merge --no-ff maBrancheFeature
```

Pour pousser sur le dépot distant toutes les branches : ```git push --all```

Pour lier une branche locale à une branche distante existante (après avoir fait un push -all créant une branche par exemple) : ```git branch --set-upstream-to origin/maBranche```

#### Ajouter un outil de DIFF pour les documents Microsoft Office
* Ajouter les lignes suivantes dans le fichier .gitconfig (vi ~/.gitconfig depuis une console gitbash) :

```
[diff]
  tool = pdiff
[difftool "pdiff"]
  cmd="wscript.exe \"c:\\Program Files\\TortoiseSVN\\Diff-Scripts\\diff-ppt.js\" \"`pwd`/$REMOTE\" \"$LOCAL\""
```
* Pour tester, exécuter la commande : ```git difftool -Y -t pdiff maPresentation.pptx```
* Ajouter un alias pour appeler ce diff (dans la liste des alias existante si besoin) :

```
[alias]
    diffP = difftool -Y -t pdiff
```
* Pour tester cet alias : ```git diffP maPresentation.pptx```

#### Ajouter deux scripts (avec alias) pour activer/désactiver un proxy
* Placer le script suivant dans le script *setGitConfigProxy.sh* dans le répertoire d'installation de GIT

```
git config --global http.proxy http://mon.proxy:8080
git config --global https.proxy https://mon.proxy:8080
```
* Placer le script suivant dans le script *unsetGitConfigProxy.sh* dans le répertoire d'installation de GIT

```
git config --global --unset http.proxy
git config --global --unset https.proxy
```
* Exécuter les commandes suivantes :

```
git config --global alias.setProxy '!sh $GIT_EXEC_PATH/../../../setGitConfigProxy.sh'
git config --global alias.unsetProxy '!sh $GIT_EXEC_PATH/../../../unsetGitConfigProxy.sh'
```

* Pour manipuler le proxy pour *NPM* et *MAVEN* depuis ces scripts, il suffit d'ajouter les lignes suivantes :

```
# setGitConfigProxy.sh
# Proxy pour GIT
git config --global --unset http.proxy
git config --global --unset https.proxy

# Proxy pour NPM
npm config rm proxy
npm config rm https-proxy

# Proxy pour MAVEN
sed "s/active>true/active>false/" -i C:/outils/apache-maven-3.5.2/conf/settings.xml```
```

```
# unsetGitConfigProxy.sh
# Proxy pour GIT
git config --global http.proxy https://mon.proxy:8080
git config --global https.proxy https://mon.proxy:8080

# Proxy pour NPM
npm config set proxy https://mon.proxy:8080
npm config set https-proxy https://mon.proxy:8080

# Proxy pour MAVEN
sed "s/active>false/active>true/" -i C:/outils/apache-maven-3.5.2/conf/settings.xml
```

#### Articles utiles :
* [merge et rebase](https://delicious-insights.com/fr/articles/bien-utiliser-git-merge-et-rebase/)
* [git diff de document MS Office](http://xcafebabe.blogspot.fr/2012/09/sexy-comparison-of-word-documents-with.html)
* [Cheat Sheet d'Atlassian](https://www.atlassian.com/git/tutorials/atlassian-git-cheatsheet)
* [Cheat Sheet de GitHub](https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf)
* [Supprimer l'historique d'un fichier contenant malencontreusement des données sensibles](https://help.github.com/articles/removing-sensitive-data-from-a-repository/)