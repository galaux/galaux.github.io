---
layout: post
title: Hudson, l'aigle de l'intégration continue
description: "Un mauvais jeu de mot de sert jamais un bon film…"
category: articles
tags: [java]
---

[Hudson](http://hudson-ci.org/) est un projet *Open Source* d'intégration continue qui s'est fait remarquer ces derniers temps par son essor fulgurant aux dépends de ses concurrents.

L'intégration continue est LE concept en vogue pour développer ses logiciels de façon professionnelle. Car que faire une fois que l'on vient de se doter d'un système de gestion de configuration et que les développeurs commencent à y remonter régulièrement leurs modifications ? Il faut automatiser le processus de *build* des WAR, des EAR, lancer des actions en fonction du succès, de l'échec, prévenir les équipes responsables du code, etc, etc. C'est exactement l'objet de l'intégration continue : faciliter l'intégration de petites parties de code en provenance de différents développeurs pour éviter les situations complexes de conflits ou de *versioning*.

Hudson fait donc tout cela : il s'installe en quelques clics sous forme de WAR, se configure via son interface WEB claire et complète et n'attend plus que des projets de développement à *builder*. Il est capable de lancer et traiter les résultats des tests unitaires ([JUnit et TestNG](http://wiki.hudson-ci.org/display/HUDSON/Meet+Hudson)) et réagit en conséquence. Qu'un build se passe mal et le développeur du code peut immédiatement être prévenu via *mail*, flux RSS ou messagerie instantanée.

Ces fonctionnalités sont assez communes chez ses concurrents comme [Apache Continuum](http://continuum.apache.org/), [Cruise Control](http://cruisecontrol.sourceforge.net/) ou [IBM Rational Build Forge](http://www-01.ibm.com/software/awdtools/buildforge/). Cependant Hudson dispose d'une fonctionnalité intéressante : il propose de répartir la charge de *build* des projets sur plusieurs machines ce qui est parfois réservé à la version payante voir tout simplement impossible avec d'autres environnements d'intégration continue.

Hudson est aussi capable de présenter différentes métriques des précédents *builds* comme par exemple le temps moyen de *build* (en rouge ceux en erreur) et bien d'autres. [![graph]({{ site.url }}/images/2009/12/graph.png)]({{ site.url }}/images/2009/12/graph.png)

Le dernier atout d'Hudson est sa conception très orientée vers les *plugins*. Même s'il ne supporte nativement que les gestionnaires de version SVN et CVS, des *plugins* existent déjà pour supporter Git, Perforce, ClearCase et bien d'autres ! Ses possibilités ne sont donc encore une fois limitées que par l'imagination de ses utilisateurs.

L'[article de Développez.com](http://linsolas.developpez.com/articles/hudson/) s'avère être une très bonne initiation technique qui peut s'utiliser en parallèle avec la [documentation officielle](http://wiki.hudson-ci.org/display/HUDSON/Meet+Hudson) claire et concise.

*Article publié sur [LinuxFr](http://linuxfr.org/~galaux/) dans le cadre de mon activité professionnelle à [Linagora](http://linagora.com/).*

