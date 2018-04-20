---
title: Créer une console Hibernate
weight: 104
---

Créer une console Hibernate (JBoss Tools) sur un projet SpringBoot+JPA utilisant une HSQLDB

## Les problèmes 
Où se trouve la difficulté ? Un peu partout en fait :

* La console Hibernate de JBoss Tools utilise encore un fichier *hibernate.cfg.xml* dont on a plus très souvent l'habitude
* La recherche d'entités par présence d'annotation n'est pas disponible (c'est Spring qui le fait d'habitude)
* il faut paramétrer correctement la connexion à une base de données HSQLDB de type FILE qui est déjà démarrée dans une application WEB

## Les solutions

Si vous voulez exécuter des requêtes SQL depuis votre Eclipse, créez-vous une connexion depuis la vue *Data Source Explorer*. Mais pensez bien à ajouter *;readonly=true;files_readonly=true;hsqldb.lock_file=false* à la fin de l'URL de connexion.

Quant à la configuration de la console Hibernate, elle demande la création de deux fichiers :

* *hibernate.properties* qui va rester vide
* *hibernate.cfg.xml* qui va contenir ce code 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration PUBLIC "-//Hibernate/Hibernate Configuration DTD 3.0//EN" "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
<session-factory name="sessionFactory">
<property name="hibernate.connection.driver_class">org.hsqldb.jdbcDriver</property>
<property name="hibernate.connection.password">login</property>
<property name="hibernate.connection.url">jdbc:hsqldb:file:C:/monCheminVersMaBaseDeDonnees/idDeMaBase;readonly=true;files_readonly=true;hsqldb.lock_file=false</property>
<property name="hibernate.connection.username">username</property>
<property name="hibernate.dialect">org.hibernate.dialect.HSQLDialect</property>
<mapping class="mon.package.MonEntite" />
<mapping class="mon.package.EtToutesMesAutresEntitesSansEnOublier" />
</session-factory>
</hibernate-configuration>
```

Une fois les fichiers créés et JBoss Tools installé sur votre Eclipse (ne cocher que Hibernate Tools durant l'installation), il vous reste à :

* ouvrir la perspective *Hibernate* (avec le petit bouton *Open a perspective* en haut à droite)
* dans la vue *Hibernate Configurations*, faire un clic-droit et un *add Configuration...*
* renseigner le formulaire avec :
 * Nom : ce que vous voulez
 * Type : *Core*
 * Hibernate version : votre version d'Hibernate
 * Project : votre projet contenant la persistance
 * Database Connection : *Hibernate configured connection*
 * Property file : le fichier précédemment créé
 * Configuration file : le fichier précédemment créé
 * Database dialect : HSQL

Après un petit clic sur la flèche à coté de votre configuration pour l'étendre, puis un clic sur *Session Factory*, vous devriez voir toutes vos classes persistantes.

Il est maintenant possible d'ouvrir un *HQL Editor* depuis un clic droit sur la configuration.

A partir du même endroit, il est possible de créer un schéma de votre mapping.