---
title: Stack Elastic
prev: /mesidees/21-jenkins/
next: /mesidees/23-gitetclefssshmultiples/

weight: 122
---

#### A quoi peut servir Elastic Stack (anciennement Stask ELK)
* obtenir rapidement les logs d'un serveur : pour éviter les demandes à l'hébergeur qui, parfois et selon les contrats, peuvent demander 3 jours pour être traitées
* corréler les logs de plusieurs serveur pour avoir une vision d'ensemble d'un processus métier : cas classique des micro-services (dont chaque composant crée ses logs) ou des systèmes SOA
* détecter des problèmes : à partir de données de référence, il est possible de détecter des anomalies (temps de traitement longs, consommation CPU ou mémoire anormale) liées à certains appels au système ou évènement métier

Dans tous les cas, avant de vouloir faire quelque chose d'intelligent avec les données collectées, il faut déjà avoir une idée des éléments à collecter :

* logs applicatifs
* logs d'erreur
* logs de requêtes HTTP
* logs de requêtes SQL
* état physique de la machine (TOP en Unix)
* ...

#### Infrastructure de la Stack Elastic

Classiquement, 3 types d'outils sont mis en oeuvre :

* les sondes qui récupèrent de l'information à leur source :
  * Kibana
  * Beats
* la base de données rassemblant toutes les informations :
  * ElasticSearch
  * Splunk 
* l'outil de visualisation des données : Kibana

D'autres outils sont aussi disponibles :

* Kafka qui sert de tampon entre les sondes et ElasticSearch permettant une indisponibilité de ce dernier (mise à jour ou maintenance diverse).
* X-Pack qui apporte à la Stack des fonctionnalités supplémentaires (sécurité, alertes, monitoring, rapports, graphiques et *machine learning*)


#### Installation sur un poste Windows

* Télécharger, depuis [le site de téléchargement d'Elastic] (https://www.elastic.co/downloads), les outils suivants (en version ZIP pour MS Windows) :
  * Kibana
  * ElasticSearch
  * LogStash
* Extraire toutes les archives 
* *En option*, installer X-Pack en exécutant les commandes :
  * depuis le répertoire d'installation d'ElasticSearch : ``` bin/elasticsearch-plugin install x-pack```
  * depuis le répertoire d'installation de Kibana : ``` bin/kibana-plugin install x-pack```
* Paramétrer les outils :
  * *En option* pour réaliser un prototype et éviter qu'ElasticSearch n'hurle en cas de problème de disque ([source](https://www.elastic.co/guide/en/elasticsearch/reference/current/disk-allocator.html)) :
    * éditer le fichier elasticsearch.yml 
	* ajouter ```cluster.routing.allocation.disk.threshold_enabled: false```
	* ou ajouter ```cluster.routing.allocation.disk.watermark.high: 99```
* Démarrer les outils :
  * depuis le répertoire d'installation d'ElasticSearch : ``` bin/elasticsearch```
  * depuis le répertoire d'installation de Kibana : ``` bin/kibana```
* Ouvrir Kibana dans [un navigateur] (http://localhost:5601) 

#### Paramétrer une configuration basique
Les configurations suivantes ont été testées pour une version particulière des outils :

* logstash-5.6.0
* kibana-5.6.0-windows-x86
* filebeat-5.6.0-windows-x86_64
* elasticsearch-5.6.0

Un fichier de logs tout bête dans *donneesSources\mesLogs.log*
```
coucou madame
bonjour monsieur
coucou monsieur
bonjour madame
hello mademoiselle
hello mademoiselle
```

Une configuration FileBeat dans *configFileBeat\mesLogs_filebeat.yml* :
```yaml
filebeat.prospectors:
- input_type: log
  # Déclaration des fichiers sources
  paths:
    - D:/Projets/Enedis/protoElk/donneesSources/mesLogs.log
  # Pour ajouter une information dans tous les logs
  fields:
   maSource: monFileBeat
output.logstash:
  hosts: ["localhost:5043"]
```

Une configuration LogStash dans *configLogStash\meslogs_pipeline.conf* :
```
input {
 # Pour prendre les éléments envoyés par Beats
 beats { port => "5043" }
}

filter {

 # Petit filtre simpliste
 grok {
  match => { "message" => '%{WORD:formulePolitesse} %{WORD:personneCiblee}' }
 }
}

output {
 # Pour logger le contenu des messages uniquement dans la console de LogStash
 #stdout { codec => plain { charset => "ISO-8859-1" } }
 # Pour logger les messages avec leurs metadata
 stdout { codec => rubydebug }
 # Pour envoyer les logs à ElasticSearch
 # @See https://www.elastic.co/guide/en/logstash/current/plugins-outputs-elasticsearch.html
 elasticsearch {
  hosts => [ "localhost:9200" ]
  # Le nom de l'index doit être en minuscule pour elasticsearch
  index => "meslogs"
  template => "D:/Projets/Enedis/protoElk/configLogStash/meslogs_template.json"
  template_name => "meslogs"
  template_overwrite => true
 }
}
```

Un template LogStash dans *configLogStash\meslogs_template.json* :
```
{
  "template": "meslogs",
  "settings": {
     "index.refresh_interval": "5s"
  },
  "mappings": {
     "_default_": {
        "dynamic_templates": [
           {
              "message_field": {
                 "mapping": {
                    "index": "analyzed",
                    "omit_norms": true,
                    "type": "string"
                 },
                 "match_mapping_type": "string",
                 "match": "message"
              }
           },
           {
              "string_fields": {
                 "mapping": {
                    "index": "analyzed",
                    "omit_norms": true,
                    "type": "string",
                    "fields": {
                       "raw": {
                          "index": "not_analyzed",
                          "ignore_above": 256,
                          "type": "string"
                       }
                    }
                 },
                 "match_mapping_type": "string",
                 "match": "*"
              }
           }
        ],
        "properties": {
           "geoip": {
              "dynamic": true,
              "properties": {
                 "location": {
                    "type": "geo_point"
                 }
              },
              "type": "object"
           },
           "@version": {
              "index": "not_analyzed",
              "type": "string"
           }
        },
        "_all": {
           "enabled": true
        }
     }
  }
}
```

Et des commandes pour démarrer les outils et nettoyer les répertoires pour répéter le test à l'infini :

* 0-resetTouteLaChaine.cmd :

``` 
rmdir filebeat-5.6.0-windows-x86_64\data /Q /S

rmdir logstash-5.6.0\data /Q /S

rmdir elasticsearch-5.6.0\data /Q /S

rmdir kibana-5.6.0-windows-x86\data /Q /S
```

* 1-elasticsearch.cmd : ``` elasticsearch-5.6.0\bin\elasticsearch ```

* 2-kibana.cmd : ``` kibana-5.6.0-windows-x86\bin\kibana```

* 3-startLogStash.cmd :

``` 
title Logstash

logstash-5.6.0\bin\logstash -f configLogStash/mesLogs_pipeline.conf --config.reload.automatic
```

* 4-startFileBeat.cmd :

``` 
title FileBeats

filebeat-5.6.0-windows-x86_64\filebeat -e -c configFileBeat/mesLogs_filebeat.yml -d "publish"
```

Une fois la première exécution réalisée sans erreur sur aucune console, sont disponibles :

* [les informations de l'index dans ElasticSearch] (http://localhost:9200/meslogs)
* [les logs dans Kibana] (http://localhost:5601/app/kibana) après avoir ajouter l'index *meslogs* (le formulaire d'ajout est affiché systématiquement si aucun index n'existe)

On obtient ainsi le tableau de bord suivant (après quelques minutes de manipulation dans Kibana) :
![elastic-kibanaExempleSimpliste](/images/elastic-kibanaExempleSimpliste.png)

#### Installation sur un Ubuntu
* Voir la [documentation officielle d'ElasticSearch] (https://www.elastic.co/guide/en/elasticsearch/reference/current/deb.html)
* Voir la [documentation officielle de Kibana] (https://www.elastic.co/guide/en/kibana/current/deb.html)
* Voir la [documentation de FileBeat] (https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-installation.html)
* Voir la [documentation de LogStash] (https://www.elastic.co/guide/en/logstash/current/installing-logstash.html)

#### Sources d'informations :
* Idées
  * [concepts généraux] (http://blog.d2-si.fr/2016/08/23/bonnes-pratiques-elastic-stack/)
  * [guide de démarrage d'Elastic] (https://www.elastic.co/fr/start)
* Outils
  * [documentation LogStash] (https://www.elastic.co/fr/products/logstash)
  * [documentation Splunk] (https://www.splunk.com/en_us/solutions/solution-areas/log-management.html)
  * [documentation Kibana] (https://www.elastic.co/fr/products/kibana)
  * [documentation Beats] (https://www.elastic.co/fr/products/beats)
  
  