---
title: Relecture, revue et audit de code
prev: /mesidees/05-retryenjava/
next: /mesidees/07-hugoetgithubpages/
weight: 106

---

#### Ma Définition

* **La relecture de code** est le travail de tout développeur juste avant de commiter son travail. Tous les outils de gestion de version permettent d'afficher le contenu du commit (*git diff* par exemple). Une relecture permet de s'assurer que les noms des composants, opérations, attributs, paramètres et variables sont explicites, que les commentaires sont utiles, ... Le développeur pourra aussi en profiter pour vérifier que le contenu du commit correspond bien aux commentaires, qu'aucun fichier ne sera intégré au commit pour un simple ajout de ligne malencontreux, ...

* **La revue de code** est un processus outillé qui permet de systématiser une relecture de code faite par un tiers entre la fin du développement et l'intégration du dit code dans les sources de l'application. Le processus peut être le suivant :
 * Dès qu'un développeur pousse son travail sur le repository, une demande de revue est envoyée à un tiers
 * Ce dernier, en cliquant sur le lien dans le mail, se voit afficher le code à auditer
 * Une liste de remarques est établie
 * Si le résultat de la revue est satisfaisant, un simple clic permet au commit d'être intégré à la branche principale
 * Si le nombre de remarques (ou leur sévérité) est trop élevé, une notification est envoyée au développeur pour qu'il corrige son code

* **L'audit de code** est un processus asynchrone qui consiste à faire une relecture de code à tout moment sur le code actuellement intégré à une branche principale. Cette pratique est tout à fait compatible avec les deux autres pratiques précédentes.

#### Le plus important
Attention, on parle ici de la qualité du code. La conception, les librairies/framework utilisés, ... sont une autre affaire !!

Quelques soient les pratiques du projet (revue ou audit ou les deux), l'élément central et primordial est l'échange entre le développeur et le relecteur. C'est cet échange qui est le garant du bon fonctionnement de ces pratiques !! Sans lui :

* le relecteur ne verra pas directement l'utilité de son travail. Il s'en lassera et fera de mauvaise relecture
* le développeur recevra une liste de remarques totalement impersonnelle. De plus, certaines remarques peuvent parfois être injustifiées. Il faut un échange verbal pour éviter d'interminables envois de mails
* le relecteur et le développeur ne pourront apprendre l'un de l'autre

#### Pour se simplifier la vie
Certains règles de développement sont aisément vérifiables (une petite expression régulière par exemple). Pourquoi ne pas utiliser un outil pour contrôler le code ? Il existe quantité d'outil de qualimétrie pour tous les langages.
Il ne remplace pas la relecture humaine mais ils peuvent la simplifier en réduisant le nombre de règles et points d'attention à contrôler.

#### Pour aller plus loin
Pour mettre en oeuvre ces bonnes idées en poussant l'interaction un cran plus loin, il faudrait que le relecteur soit à côté du développeur pour lui réfléchir avec lui. Le relecteur aurait alors la possibilité de faire des remarques de conception aussi. C'est du **pair programming** !

#### Sources
* [5 reasons you are not doing code reviews] (http://blog.8thcolor.com/en/2014/04/5-reasons-you-are-not-doing-code-reviews/)
* [Pairing vs code review] (http://phinze.github.io/2013/12/08/pairing-vs-code-review.html)

