---
layout: post
title: Jonas - présentation d'un serveur d'application Java EE
description: ""
category: articles
tags: [java,server]
---

![logo\_jonas3](http://08000linux.com/blogs/files/2009/10/logo_jonas3.jpg) Les développeurs de serveurs d'application Java sont décidément inspirés par le monde de la mer puisque qu'après [Glassfish](https://glassfish.dev.java.net/) de SUN, voici [JOnAS](http://wiki.jonas.ow2.org/xwiki/bin/view/Main/). Cette fois-ci, derrière le produit ne se profile pas une mais plusieurs sociétés regroupées au sein de l'[OW2 Consortium](http://www.ow2.org/) (dont le but est la promotion de logiciels Open Source de middleware). On trouvera parmi elles de grands noms comme Bull, France Telecom, Alcatel Lucent, l'INRIA ou encore Red Hat et [bien d'autres](http://www.ow2.org/view/MembershipJoining/ConsortiumMembers). Le développement de JOnAS a été initié en 1998 et il est certifié Java EE 5 depuis sa version 5.1 de Septembre 2009. C'est donc un produit ouvert sous licence GNU LGPL, mature et qui a sur le papier toute les qualités pour trouver le chemin de serveurs de production. Une des spécificités de JOnAS est qu'il est construit sur une architecture [OSGI](http://www.osgi.org/). Son but est de fournir les spécifications d'une plate-forme de services Java. Comme JOnAS répond à ces spécifications, n'importe quel développeur habitué au découpage logiciel OSGI retrouvera ses petits en un clin d'oeil. Voyez plutôt le schéma d'agencement de la bête : [![jonas-5-schema](http://08000linux.com/blogs/files/2009/10/jonas-5-schema.png)](http://08000linux.com/blogs/files/2009/10/jonas-5-schema.png) L'architecture OSGI permet de retrouver les composants standards décrits dans le schéma précédent. Ceux-ci sont modulaires et extensibles. Ils peuvent aussi être configurés dynamiquement pendant l'exécution (*runtime*).

JOnAS repose sur des composants bien connus pour certaines de ses fonctionnalités. La partie EJB3 est ainsi prise en charge par [EasyBeans](http://www.easybeans.net/) et le Java Messaging Service (JMS) est supporté par [JORAM](http://joram.ow2.org/). De plus les applications web pourront être déployées avec [Tomcat](http://tomcat.apache.org/) ou [Jetty](http://www.mortbay.org/jetty/).

Une des fonctionnalités importante de tout bon serveur d'application est sa capacité à fonctionner en cluster. Pas d'inquiétudes pour JOnAS puisqu'il propose de gérer le fonctionnement en grappe au niveau HTTP et EJB ce qui est bien appréciable pour les applications les plus gourmandes. Voici d'ailleurs la page qui décrit plus en détails [les possibilités de ce serveur](http://wiki.jonas.ow2.org/xwiki/bin/view/Main/JOnAS5). Toutes ces fonctionnalités sont bien évidemment administrables à distance en particulier via les habituelles consoles web et JMX. Cependant l'OW2 Consortium réserve une surprise bien appréciable dans ce domaine : [JASMINe](http://wiki.jasmine.ow2.org/xwiki/bin/view/Main/WebHome), un outil ouvert d'administration de serveurs d'application.

*Article publié sur [LinuxFr](http://linuxfr.org/~galaux/) dans le cadre de mon activité professionnelle à [Linagora](http://linagora.com/).*

