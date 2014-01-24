---
layout: post
title: Terracotta monte les JVM en grappe
description: "Clustering de JVM au programme"
category: articles
tags: [java,administration]
---

![logo](http://08000linux.com/blogs/files/2009/11/logo.png)

Pour satisfaire les besoins des applications JavaEE modernes, le recours au *clustering* est souvent indispensable. Celui-ci vise à augmenter les ressources allouées aux applications en multipliant les serveurs à sa disposition. Les solutions de *clustering* de serveurs d'application et de bases de données sont légions dans les environnements techniques des entreprises modernes mais qu'en est-il du clustering de JVM?

C'est la mission que s'est donnée [Terracotta](http://www.terracotta.org/) avec son outil OpenTerracotta. L'idée derrière le concept de Terracotta est de *descendre* la mise en grappe d'un cran pour l'appliquer directement aux machines virtuelles Java. Ceci permet de faire fonctionner une application trop gourmande pour une seule JVM sur plusieurs d'entre-elles de façon complètement transparente pour l'applicatif. De cette manière, le développeur est débarrassé de toute préoccupation de programmation en grappe et peut ainsi se consacrer pleinement à son application dont la conception est de fait simplifiée.
 L'architecture d'une telle solution est classique quand il s'agit de *clustering* : un serveur, des clients et (moins courant) un serveur de stockage.

[![terracotta\_architecture](http://08000linux.com/blogs/files/2009/11/terracotta_architecture.png)](http://08000linux.com/blogs/files/2009/11/terracotta_architecture.png)

Les clients accueillent chacun une JVM qui inclut au démarrage un ensemble d'API Terracotta. Leur rôle est d'intercepter les appels Java responsables de la synchronisation des processus est d'y injecter du *bytecode* afin d'informer le serveur Terracotta des changements effectués sur le tas (*heap*) et donc les altérations subies par les objets. Les composants Terracotta répartis ne s'échangent que des deltas sur les objets des tas et économise ainsi la bande passante. Toutes les données qui concernent les objets Java sont enfin conservées par le serveur de stockage. Dans les faits, Terracotta trouve quatre champs d'applications majeurs :

-   Réplication de sessions HTTP (Tomcat, Oracle WebLogic, Struts, Spring, ...)
-   Cache Distribué
-   *Clustering* de POJO/Intégration Spring
-   Collaboration, Coordination, et gestion d'évènements

Pour une présentation plus détaillée de cet outil ambitieux qu'est Terracotta, se référer à [cet autre article](http://blog.zenika.com/index.php?post/2009/05/05/Présentation-de-Terracotta2) du blog Zenika.com. On ne pourra ensuite que citer les très bons et très techniques articles en anglais [d'infoq.com](http://www.infoq.com/articles/open-terracotta-intro) et de [javaworld.com](http://www.javaworld.com/javaworld/jw-01-2009/jw-01-osjp-terracotta.html) pour dévoiler les derniers secrets de l'outil.

*Article publié sur [LinuxFr](http://linuxfr.org/~galaux) dans le cadre de mon activité professionnelle à [Linagora](http://linagora.com/).*

