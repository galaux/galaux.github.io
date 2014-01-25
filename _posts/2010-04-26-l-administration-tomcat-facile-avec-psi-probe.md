---
layout: post
title: L'administration Tomcat facile avec PSI-probe
description: "J'ai des webapps c'est bien mais comment les administrer?"
category: articles
tags: [java,administration]
---

[Tomcat](http://tomcat.apache.org/), le serveur web (conteneur de servlets) bien connu de la fondation Apache séduit par sa robustesse et sa simplicité. Robuste, simple, fiable, clusterisable, tout ça est très bien mais qu'en est il de la supervision ? Il est essentiel pour des équipes d'exploitation de garder une vue des métriques vitales des serveurs qu'elles administrent surtout en ces temps de *clustering,* qui rajoute une couche de complexité. Petit retour sur un projet qui renait de ses centres, pour garder la main sur Tomcat donc, rien de tel que [PSI-probe](http://code.google.com/p/psi-probe/).

L'historique de l'outil est tumultueux : initié sous le nom de "Tomcat-Probe", le projet a été renomé en [Lambda-probe](http://www.lambdaprobe.org/) avant de rentrer dans une phase d'inactivité encore effective aujourd'hui. Alors Lambda-probe, projet mort? C'était sans compter sur la force des communautés *Open Source*. Sans nouvelles du développeur et confortés par la licence GPL, des utilisateurs du forum ont repris le développement de l'outil d'administration de serveur Tomcat en le *forkant* : PSI-probe.

Les mesures proposées par PSI-probe dans sa version 2.0.2 sont très diverses :

- gestion des applications déployées (affichage des sessions et de leurs détails, désactivation de celles-ci), de leurs composantes (connexions JDBC)
- gestion des *datasources*, visualisation de leurs taux d'engorgement
- déploiement d'applications
- affichage des *logs* Tomcat et des logs applicatifs
- informations sur les processus en cours
- gestion en *clusters*
- informations système de l'hôte
- état du serveur

La liste impressionantes de ses possibilités par rôle est à consulter [sur le site](http://code.google.com/p/psi-probe/wiki/Features).

PSI-probe gère en effet les utilisateurs en plusieurs rôles ce qui devrait intéresser les environnements fortement industrialisés dans lesquels plusieurs utilisateurs d'équipes différentes doivent accéder à certaines statistiques ou actions selon leurs fonctions. Les développeurs ont mis l'accent sur la gestion des instances de serveurs en *cluster* qui est implémentée nativement.

Le logiciel est proposé sous forme d'application *web* qui se déploie naturellement sous Tomcat mais aussi sous JBoss de façon triviale. L'ancien site de LambdaProbe propose des [captures d'écran](http://www.lambdaprobe.org/d/screenshots.shtml) et une [démonstration en ligne](http://demo.lambdaprobe.org/probe/index.htm) (d'une ancienne version 1.7) qui donnent une bonne idée des mesures déjà disponibles à l'époque ainsi que de l'interface utilisateur ergonomique, claire et complète que l'on peut retrouver en français. Pour info, PSI-probe nécessite une exécution en environnement privilégié afin de pouvoir administrer les WARs de son serveur ainsi que de l'activation de la console JMX dans la machine virtuelle Java.

[![charts]({{ site.url }}/images/2010/04/charts.png)]({{ site.url }}/images/2010/04/charts.png)

Comme on a pu le voir au moment de l'inactivité du projet, [le forum](http://www.lambdaprobe.org/forum2/index.jspa), véritable ligne de vie d'un logiciel Open Source, est actif et les demandes d'informations ne restent pas lettre morte. Les [idées de développement futurs](http://code.google.com/p/psi-probe/wiki/DevelopmentRoadmap) ne manquent pas comme par exemple [le support de Tomcat 6](http://code.google.com/p/psi-probe/issues/list?can=2&q=Milestone:2.2) ou les avertissements par email. PSI-probe est le parfait exemple de projet Open Source : basé sur une communauté active et qualifiée, à même de reprendre le projet de bout en bout, et de proposer une réponse pertinente à un besoin souvent exprimé.

PSI-probe? Un incontournable!

*Article publié sur [LinuxFr](http://linuxfr.org/~galaux/) dans le cadre de mon activité professionnelle à [Linagora](http://linagora.com/).*
