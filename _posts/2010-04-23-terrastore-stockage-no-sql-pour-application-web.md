---
layout: post
title: Terrastore - stockage No-SQL pour application web
description: ""
category: articles
tags: [database,java,development]
---

Plus besoin de les présenter, les FaceBook, twitter, etc. sont les applications qui font le web d'aujourd'hui ! Mais s'ils est évident de voir les nouveaux usages qu'elle apportent, qu'en est il côté techno ? Cette nouvelle race d'applications se distingue des applications *web* plus traditionnelles par le format des données qu'elles manipulent, fini les données fortements structurées ou les schémas fixes : les applications du *web* de demain veulent de la flexibilité, de l'évolutivité, de la scalabilité et les outils qui vont avec. Et c'est exactement le créneau de [TerraStore](http://code.google.com/p/terrastore/), un entrepôt de documents au format [JSON](http://fr.wikipedia.org/wiki/JavaScript_Object_Notation).

TerraStore se positionne dans la même catégorie que [CouchDB](http://couchdb.apache.org/) et [Cassandra](http://cassandra.apache.org/) de la fondation Apache, même si ces dernier endosse plus franchement le rôle de base de donnée. Les fonctionnalités que TerraStore propose sont celles d'un véritable entrepôt de données textuelles accessibles directement en HTTP ce qui justifie son intérêt pour des applications massivement en-ligne. On plonge là au coeur du thème très en vogue du [NO-SQL](http://fr.wikipedia.org/wiki/NoSQL). L'application est, de plus conçue, sur une architecture en *cluster* afin d'assurer un service sous fortes charges. Deux types de composants sont présents : le *server* qui stocke directement les données ou plutôt leurs fragments et le *master*, responsable de la répartition des données et de la charge sur les *servers*. Une installations de TerraStore suit donc l'habituel schéma des serveurs à l'écoute du ou des maîtres qui gère(nt) l'application en grappe. L'accent est aussi mis sur les performance réseau : les traitements sur les données sont ainsi effectuées sur le serveur même qui héberge les données. Encore un point essentiel pour des applications fortement orientées *web*.

Le nom de TerraStore témoigne de l'utilisation sous-jacente de [Terracotta](http://www.terracotta.org/), cette solution *Open Source* de *clustering* de JVM qui avait déjà fait l'objet d'un article. Malgré cet empilement de couches, TerraStore reste d'une simplicité d'installation et d'utilisation assez déconcertante comme en témoigne [ce tutoriel de quelques lignes](http://code.google.com/p/terrastore/wiki/Getting_Started) qui met en place un *master* et deux *servers* via [Apache Ant](http://ant.apache.org/). Malgré sa jeunesse (version 0.4.2 en téléchargement), TerraStore présente des fonctionnalités visiblement très abouties : possibilité de monitoring via console JMX, fonctionnalités de sauvegarde des *servers* et aussi capacité de gestion orientée évènements.

Les ambitions de solutions telles que CouchDB et TerraStore ne sont peut-être pas de révolutionner le monde des bases de données mais l'effort reste très intéressant et prometteur. Ces solutions sont donc à surveiller car elles pourraient être une autre preuve de la théorie de l'évolution mais cette fois-ci dans le domaine de l'informatique.

*Article publié sur [LinuxFr](http://linuxfr.org/~galaux/) dans le cadre de mon activité professionnelle à [Linagora](http://linagora.com/).*

