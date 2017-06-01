#!groovy

// Define job properties
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([])])

pipeline {

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

                // télécharge les dépendances
				sh "npm install"

    			// index
				// /!\ la commande node a été renommée en nodejs
				sh "sed -i 's/node/nodejs/' ./node_modules/lunr-hugo/bin/index.js"
				sh "npm run index"

				// build
				sh "chmod u+x ./hugo"
				sh "npm run build"
				
				stash name: 'public', includes: 'public/**/*'
			}
		}

		stage ('Deploy') {
			agent any
			steps {

				// Suppression du répertoire 'public'
				dir('public') {
					deleteDir()
				}

				// Clone du repository dans lequel doit être placé le site généré
				sh "git submodule add -f -b master https://github.com/talbotgui/talbotgui.github.io.git public"

				// Mise en place du contenu du site dans le répertoire
				//sh "rm -rf public/*"
				unstash 'public'
				
				// commit et push
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
