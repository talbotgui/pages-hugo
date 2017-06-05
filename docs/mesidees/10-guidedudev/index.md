---
title: Guide Du Développeur
prev: /mesidees/09-objectmother/
next: /mesidees/11-programmationdefensive/

weight: 110
---

Le Guide Du Développeur est l'espace de partage d'une équipe. Il contient toutes les informations utiles aux membres de l'équipe.

Chaque question posée par un collaborateur doit donner lieu à un nouveau paragraphe dans le GDD.


**Il est inutile d'imposer un guide du développeur.** Il faut le proposer et le faire adopter. C'est un outil pour l'équipe construit par l'équipe. Si personne ne le consulte avant de se mettre en quête d'une réponse (sur Internet ou au près d'un leader), alors il est inutile.


## Astuces

La première astuce est à destination des Lead (les membres de l'équipe qui ont tout dans la tête). Dès qu'un collaborateur vient vous poser une question, demandez-lui s'il a consulté le GDD. Si ce n'est pas le cas, qu'il retourne à son poste pour vérifier si la réponse n'est pas dans le GDD (même si vous savez que ce n'est pas le cas). Il faut que cela devienne un réflexe pour toute l'équipe. Et quand il reviendra vous poser la question et qu'il aura obtenu sa réponse, dîtes lui d'ajouter un paragraphe dans le GDD !!

Bien souvent, à ce moment-là, le collaborateur vous dit qu'il n'a pas le temps. Pour beaucoup de gens, toutes les tâches sont prioritaires à la documentation (même le café). Mais il est vrai qu'on n'a pas toujours le temps. Pas de soucis, il vous suffit d'un tableau blanc (ou d'un paperBoard). A chaque réponse obtenue, le collaborateur inscrit son prénom ainsi que les mots clefs de sa question et de sa réponse. Charge à lui de remplir le GDD dans un délai raisonnable puis de venir effacer (ou barrer) son nom.

Valoriser les contributeurs est aussi une bonne idée. Bien souvent, le dernier arrivé sur le projet est le principal contributeur. Il est aussi celui qui doit s'approprier le GDD. Un petit mail avec le meilleur contributeur du mois peut aider à faire adopter le GDD.

Question outil, un fichier MS Word sur un serveur FTP est une solution à proscrire car l'édition à plusieurs est impossible et la gestion des versions (avec un cartouche, ...) est lourde. Un document partagé dans une bonne GED (Gestion Electronique de Documents) est une solution. Mais le plus simple reste un WIKI ou un site statique généré (comme le présent site avec Hugo). Les fonctions nécessaires sont : 

* pas de prise de tête avec la mise en forme
* suivi des modifications et consultation possible de l'historique
* simplicité de déplacement des paragraphes (pour permettre la réorganisation au fur et à mesure que le GDD s'étoffe).
* dans tous les cas :
  * l'usage de l'outil doit être simple
  * un moteur de recherche doit être présent
  * aucun workflow complexe (relecture, validation, ...) ne doit freiner l'enrichissement du GDD

Enfin, il n'existe aucun modèle de GDD. Chaque GDD est unique car les préoccupations, les questions et les réponses changent d'un projet à un autre (technologies différentes, clients différents, organisations différentes, équipes différentes). Il est préférable de démarrer avec une simple page faisant office de foire aux questions. Quand la page devient indigeste, il suffit de regrouper les questions par thème puis de les extraire dans des pages différentes. Ainsi le GDD émergera petit à petit.


## Idées de sujets à intégrer au GDD :
**Ceci n'est pas un menu de GDD mais juste une liste de sujets à traiter sur la plus part des projets ! Votre GDD, vos questions, votre sommaire !!!**

* référence documentaire
 * cahier des charges
 * dossier(s) d'architecture
 * référentiel des exigences
 * référentiel des tests

* exigences
 * quel est le niveau de détails des exigences en entrée des développements ?
 * quel est le contexte du projet ou de l'application ?

* test
 * quel est contractuellement le niveau de détails et le formalisme à livrer en fin de conception ?
 * stratégie de test du projet
 * types de tests disponibles
 * cas de test devant être validés en fin de développement
 * cas de test devant être codés dans un composant de test (avec sa granularité et les outils à utiliser)
 
* conception
 * résumé de l'architecture technique
 * quels sont les impacts de l’AT sur la conception (BI, ERP ou détails trop fins en xNet) ?
 * quel est le niveau de détails attendu en fin de conception (peut-être besoin d’être plus précis que le niveau contractuel en fonction du contexte) ?
 * quels sont les outils de conception et/ou de modélisation ?
 * quels sont les objectifs des modèles (compréhension, communication, …) et quelle est leur durée de vie ?
 * règles de conception (programmation défensive, @designByContrat, modularité, ...)
 * jeu de données de test

* développement
 * règles de nommage
 * liste des frameworks disponibles, leur usage et le pointeur sur leur documentation

* industrialisation
 * éléments composant la Gestion de Configuration Logicielle (Git, SVN, HP ALM, Archiva, Jira, ...)
 * règles d'usage de la GCL (flow GIT/SVN, snapshots dans le repository de binaires, ...)
 * règles d'usage de l'intégration continue
 * organisation des équipes en amont et en aval des développements (Continuous Delivery)

* environnements
 * installation du poste de développement
 * liste et paramètres des environnements de test
 * liste des environnements du client
