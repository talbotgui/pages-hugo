---
title: Hugo et GitHub:Pages
weight: 107
---

## Démarrage

Les objectifs sont :

* de disposer d'un repository contenant les sources du site avec des pages en markdown
* d'utiliser [Hugo] (http://gohugo.io/) pour réaliser la mise en page et générer le site statique 
* de déployer ce site statique dans un repository [GitHub:Pages] (https://pages.github.com/)
* d'automatiser complètement la génération et le déploiement du site

Le point de départ :

* le site existe déjà (cf. [créer un site avec Hugo] (/mesidees/08-creationavechugo/))
* Hugo génère le site dans le répertoire 'public' (répertoire par défaut)
* les sources du site sont dans un repository Git
* un serveur Jenkins est disponible
* Jenkins contient une configuration login/password avec le compte GitHub

Les valeurs utilisées ici sont celle du site que vous consultez en ce moment :

* [le repository des sources] (https://github.com/talbotgui/pages-hugo) : https://github.com/talbotgui/pages-hugo.git
* [le repository d'hébergement] (https://github.com/talbotgui/talbotgui.github.io) : https://github.com/talbotgui/talbotgui.github.io.git
* la clef du credential dans Jenkins : compteGithub
* le nom de l'utilisateur qui apparait dans l'historique GitHub du repository d'hébergement : Jenkins de Guillaume TALBOT

## Etapes
* Dans Jenkins, créer un job MultiBranchPipeline pointant sur votre repository GitHub
* Sur le serveur Jenkins, installer l'outil [NPM] (https://docs.npmjs.com/getting-started/installing-node)
* A la racine du repository des sources, placer le Jenkinsfile suivant :

```groovy
#!groovy

// Définition des propriétés du job : on ne garde que les 5 derniers build
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([])])

pipeline {

  // aucun agent par défaut
  agent none

  stages {
    
    stage ('Checkout') {
      agent any
      steps {
      
        // clone des sources du site
        git url: 'https://github.com/talbotgui/pages-hugo.git'
        
        // clone du thème HUGO
        dir('themes') {
            sh "git clone https://github.com/matcornic/hugo-theme-learn.git"
        }
        
        stash name: 'sources', includes: '*'
      }
    }

    stage ('Build') {
      agent any
      steps {
        unstash 'sources'

        // génère le site
        sh "chmod u+x ./hugo"
        sh "./hugo -t hugo-theme-learn"
        
        stash name: 'public', includes: 'public/**/*'
      }
    }

    stage ('Deploy') {
      agent any
      steps {

        // supprime le répertoire 'public'
        dir('public') {
          deleteDir()
        }

        // clone le repository d'hébergement dans le répertoire 'public'
        sh "git submodule add -f -b master https://github.com/talbotgui/talbotgui.github.io.git public"

        // place le site généré dans le répertoire du repo
        sh "rm -rf public/*"
        unstash 'public'
        
        // commit et push avec les paramètres de compte 'compteGithub' (paramétrage Jenkins)
        dir('public') {
          withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'compteGithub', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD']]) {
            sh "git config user.name 'Jenkins de Guillaume TALBOT'"
            sh "git add -A"
            sh "git commit -m 'Publication du site'"
            sh('git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/talbotgui/talbotgui.github.io.git')
          }
        }
      }
    }
  }
  
  post {
        //success {}
        //unstable {}
        //failure {}
        always {
      node ('') { step([$class: 'WsCleanup', notFailBuild: true]) }
        }
        //changed {}
    }
}

```