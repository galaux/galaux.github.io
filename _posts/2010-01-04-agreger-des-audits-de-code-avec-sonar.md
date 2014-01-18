---
layout: post
title: Agréger des audits de code avec Sonar
description: ""
category: articles
tags: [java,tests,development]
---

Dans le monde du développement Java, on ne compte plus les outils de revue de code, d'audit ou de détection d'anomalies. Il devient dangereux de s'en passer pour un projet de développement de grande envergure. Ils sont autant de garde-fous qui proposent un angle de vue propre. Cependant, les installer, les paramétrer et interpréter leurs résultats est souvent complexe et prend du temps ! C'est là qu'est la plus-value de [Sonar](http://sonar.codehaus.org/) : plus besoin de considérer les outils d'audit un par un, Sonar, logiciel Open Source de mesure de qualité de code propose de jouer le rôle d'interface.

Pour mieux comprendre son mode de fonctionnement : voyons-en l'architecture. Une fois installé et lancé, Sonar fournit une interface web qui propose les habituelles pages de configuration sur le port 9000 de la machine hôte. C'est ici que se fait le paramétrage des outils sous-jacents que l'on souhaite utiliser. La capture suivante montre la fine granularité de configuration mise à disposition. [![01\_config](http://08000linux.com/blogs/files/2009/12/01_config1.png)](http://08000linux.com/blogs/files/2009/12/01_config1.png)

Sonar recoupe les différents vérificateurs de code et analyse ainsi le code selon 6 axes :

-   identification des duplications (CPD, PMD)
-   mesure du niveau de documentation
-   respect des règles de programmation (Checkstyle, PMD)
-   détection des bugs potentiels (FindBugs)
-   évaluation de la couverture de code par les tests unitaires (Cobertura, Clover, Emma, JUnit, Surefire)
-   analyse de la répartition de la complexité

Une fois la configuration faite, les utilisateurs de Maven ne seront plus qu'à une commande de générer automatiquement tous les précieux rapports d'audit : `mvn sonar:sonar` Pour les projets qui ne seraient pas encore "mavenisés", Sonar propose [un tutoriel de quelques lignes](http://docs.codehaus.org/display/SONAR/Collect+data#Collectdata-NonMavenprojects%28sonarlightmode%29) pour remédier au problème.

Toutes les informations collectées par Sonar et stockées dans sa base sont consultables via l'interface web. On y retrouvera donc clairement présentées toutes les métriques des outils sous-jacents utilisés comme on peut le voir dans les captures suivantes. [![02\_stats\_01](http://08000linux.com/blogs/files/2009/12/02_stats_01.png)](http://08000linux.com/blogs/files/2009/12/02_stats_01.png) [![03\_stats\_02](http://08000linux.com/blogs/files/2009/12/03_stats_02.png)](http://08000linux.com/blogs/files/2009/12/03_stats_02.png)

Sonar s'axe sur une politique de [plugins](http://docs.codehaus.org/display/SONAR/Sonar+Plugin+Library/) qui élargit nettement le champ des outils d'audit supportés. On pourra donc s'appuyer sur ceux existant ou se lancer dans le codage d'un nouveau si le besoin s'en fait sentir. Pour plus d'information sur Sonar, se référer à la [documentation officielle](http://sonar.codehaus.org/documentation/) très fournie ou à l'[article de developpez.com](http://linsolas.developpez.com/articles/java/qualite/sonar/?page=page_7)

*Article publié sur [LinuxFr](http://linuxfr.org/~galaux/) dans le cadre de mon activité professionnelle à [Linagora](http://linagora.com/).*

