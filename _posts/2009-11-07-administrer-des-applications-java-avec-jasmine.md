---
layout: post
title: Administrer des applications Java avec JASMINe
description: "Quelles application utiliser pour administrer ses webapps Java EE"
category: articles
tags: [java,administration]
---

![jasmine](http://08000linux.com/blogs/files/2009/11/jasmine.png)L'[OW2 Consortium](http://www.ow2.org/) fait déjà beaucoup parler de lui avec [JOnAS](http://wiki.jonas.ow2.org/xwiki/bin/view/Main/WebHome), son serveur d'application Java Open Source. Mais comme nous l'avons dit dans [un article précédent]({{ site.url }}/articles/glassfish-serveur-d-application-java-ee/), il y a beaucoup plus qu'un seul projet dans le sac de ce consortium décidément bien inspiré !

Voici donc [JASMINe](http://wiki.jasmine.ow2.org/xwiki/bin/view/Main/WebHome), un outil dédié à l'administration de cluster JavaEE et de plateformes d'architectures orientées services (SOA). Le constat de base de ses concepteurs est simple : les architectures JavaEE modernes sont toujours plus compliquées. Il devient facile de se perdre dans les fichiers de configuration des différents composants et ceci n'est évidemment pas arrangé par l'utilisation de clusters. Il est donc temps de sortir la tête du guidon et de rechercher une vision plus élevée de l'architecture, avec une interface graphique par exemple. C'est exactement ce que cet outil propose de faciliter à travers sa conception modulaire.

[JASMINe Design](http://wiki.jasmine.ow2.org/xwiki/bin/view/Main/Design) est l'application qui propose l'interface graphique de conception et configuration d'architectures middleware. Une fois installée, de simples clics permettent de créer des architectures et de les voir vivre sous nos yeux. Si les serveurs sont déjà déployés il est possible de créer ces schémas et de les rattacher à l'existant. On peut aussi voir dans le [roadmap](http://wiki.jasmine.ow2.org/xwiki/bin/view/Main/Roadmap) une fonctionnalité de découverte de l'architecture qui devrait être très appréciable.

[![JASMINe-cluster-JOnAS](http://08000linux.com/blogs/files/2009/11/JASMINe-cluster-JOnAS1.png)](http://08000linux.com/blogs/files/2009/11/JASMINe-cluster-JOnAS1.png)

Une fois la topologie d'une architecture créée, rien de plus facile que de la mettre en œuvre grâce à [JASMINe Deploy](http://wiki.jasmine.ow2.org/xwiki/bin/view/Main/Deploy). [JaDOrT](http://wiki.jasmine.ow2.org/xwiki/bin/view/Main/Deploy#jadort) (*JASMINe Deployment Orchestration Tool*) est le module qui permet le déploiement, la maintenance des JOnAS, [Glassfish](https://glassfish.dev.java.net/) et autres mais aussi des machines virtuelles. Pour ceux qui sont attachés à la ligne de commande, [DeployME](http://wiki.jasmine.ow2.org/xwiki/bin/view/Main/Deploy#deployme) est l'interlocuteur idéal pour déployer le produit de notre travail avec le GUI JASMINe Design. [Jade](http://wiki.jasmine.ow2.org/xwiki/bin/view/Main/Deploy#jade) est quant à lui un projet plus poussé dont le but est de fournir des fonctionnalités d'auto-gestion des applications. Même si on sort sans doute ici du cadre de la production, le travail vaut la peine d'être cité.

[![JaDOrT-upload](http://08000linux.com/blogs/files/2009/11/JaDOrT-upload1.png)](http://08000linux.com/blogs/files/2009/11/JaDOrT-upload1.png)

La gestion de la supervision des architectures ainsi créées est dévolue à [JASMINe Monitoring](http://wiki.jasmine.ow2.org/xwiki/bin/view/Main/Monitoring) via les habituelles console JMX, console web etc.

Le [site officiel de JASMINe](http://wiki.jasmine.ow2.org/xwiki/bin/view/Main/#modules) permet de connaître la liste des composants middleware ainsi que les versions supportées par les différents modules. Je ne peux ensuite que vous conseiller les [screencasts de démonstration des fonctionnalités](http://wiki.jasmine.ow2.org/xwiki/bin/view/Main/Demos) qui sont le meilleur moyen pour se faire une idée plus précise des possibilités de cet outil très prometteur.

*Article publié sur [LinuxFr](http://linuxfr.org/~galaux/) dans le cadre de mon activité professionnelle à [Linagora](http://linagora.com/).*

