---
layout: post
title: Le conteneur de servlet Jetty
description: ""
category: articles
tags: [java,server]
---

![JETTY](http://08000linux.com/blogs/files/2009/12/JETTY.gif) Dans la grande famille des serveurs Java, plusieurs objectifs se côtoient. Certains comme JBoss ou Glassfish jouent la carte de l'exhaustivité dans l'implémentation des fonctionnalités alors que d'autres se spécialisent. Parmi ces derniers, le serveur [Jetty](http://www.mortbay.org/jetty/) se distingue, lui, par son efficacité.

Avant toute chose, posons des bases saines : Jetty n'est pas exactement un serveur d'applications Java. En effet, il ne propose pas toutes les fonctionnalités nécessaires pour être appelé ainsi. Il n'y a par exemple pas de prise en charge native de la norme EJB : ceci peut être dévolu à des projets tiers comme par exemple [OpenEJB](http://openejb.apache.org/) ou [EasyBeans](http://www.easybeans.org/GettingStarted/GettingStartedJetty.html) en fonction des besoins de l'application cible. Jetty assume le rôle de serveur HTTP et de conteneur de servlets Java ce qui revient à une version très allégée d'un serveur d'applications. Cette particularité permet à Jetty de jouer la carte de la légèreté et de l'adaptabilité : après tout, rares sont les serveurs java qui peuvent se vanter à la fois d'être [embarqués dans des téléphones](http://code.google.com/p/i-jetty/), des [systèmes distribués](http://docs.codehaus.org/display/JETTY/Jetty+Powered/#JettyPowered-Hadoop) et participer à des [programmes de calculs de sondes sur Mars](http://docs.codehaus.org/display/JETTY/Jetty+Powered/#JettyPowered-JPL) !

Sur Terre, Jetty s'illustre par sa rapidité de lancement ainsi que par sa capacité à recharger des pages modifiées à chaud. Cette fonctionnalité est particulièrement utile aux développeurs qui n'ont de fait plus besoin de redémarrer leur serveur de test à chaque modification mineure d'une page. Jetty dispose d'une intégration native dans Maven ce qui fait de lui un vrai bonheur quand il s'agit de l'ajouter à un projet Java comme on peut le voir dans certains [tutoriels](http://www.tomsquest.com/blog/jetty-demarrage-rapide/). L'intéressé est si léger qu'il a même été embarqué directement dans un [plugin Eclipse](http://code.google.com/p/run-jetty-run/) ce qui résume son déploiement à trois clics dans le gestionnaire de mises à jour d'Eclipse via l'URL *http://run-jetty-run.googlecode.com/svn/trunk/updatesite*.

Voilà donc, dans la longue liste des serveurs Java, un exemple intéressant d'une serveur qui a su tirer son épingle du jeu avec une politique de niche. A la vue de sa success story, on peut se dire que c'est une recette viable !

*Article publié sur [LinuxFr](http://linuxfr.org/~galaux/) dans le cadre de mon activité professionnelle à [Linagora](http://linagora.com/).*

