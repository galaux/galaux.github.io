---
layout: post
title: Gérer les Thread Dump Java
description: "Les outils d'exploitation de thread dump Java"
category: articles
tags: [java,development]
---

L'une des forces du Java est d'être entouré d'outils performants et efficaces pour les développeurs. Nous parlons ici des *frameworks* de haut niveau qui viennent décharger le développeur de beaucoup de préoccupations de base, comme par exemple, la gestion de la mémoire ou des processus. Pourtant quand une application rend la machine virtuelle instable, il faut bien descendre de cette tour d'ivoire et savoir (re)mettre les mains dans le cambouis! Analyser l'état des processus présents dans la JVM en cas d'engorgement ou de *crash* permet de comprendre quels sont les appels qui ont consommé les ressources de façon anormale. Voici un rapide tour de quelques outils uiles dans ces moments là...

### JConsole
Disponible dans le JDK de SUN et développée pour centraliser les outils d'administration de JVM, [JConsole](http://java.sun.com/developer/technicalArticles/J2SE/jconsole.html) se connecte à une machine virtuelle locale ou distante. Son interface graphique présente l'utilisation de la mémoire, le nombre de *threads*, de classes dans la *heap* mais permet aussi (pour les JVM les plus récentes) de générer des *dumps* de la *heap* et des *threads* courants. Pour cela il suffit d'accéder à l'onglet MBeans, "java.lang \> Threading \> Operations" et de cliquer sur "dumpAllThreads". On accède ainsi aux détails des processus en cours, en attente ainsi qu'à leur état.

![jconsole]({{ site.url }}/images/2009/12/jconsole.png)

### VisualVM
Voici un autre [outil SUN](https://visualvm.dev.java.net/) plus récent et ergonomique que l'austère JConsole. Les informations présentées sont aussi plus détaillées : on retrouve par exemple un (rapide) historique de l'état de tous les *threads*. La force de VirtualVM repose en partie sur son fonctionnement en *plugins* qui lui permet de coller au plus près aux applications inspectées (Glassfish, OSGi, KillApplication, etc). [![visualvm]({{ site.url }}/images/2009/12/visualvm1.png)]({{ site.url }}/images/2009/12/visualvm1.png)

### Thread Dump Analyze
Voilà un *plugin* Eclipse : [Thread Dump Analyzer](http://lockness.plugin.free.fr/home.php) qui agence les sorties textuelles des *dumps* que l'on peut récupérer dans les *logs* des serveurs d'application. Le *plugin* ne fait rien d'extraordinaire mais permet d'accéder rapidement aux informations essentielles dans des *logs* qui sont souvent bavards. [![ETDA]({{ site.url }}/images/2009/12/ETDA.png)]({{ site.url }}/images/2009/12/ETDA.png)

Ces outils sont la partie émergée de l'iceberg que constituent des applications en ligne de commande comme jps, jstat, jdebug, etc. Si l'environnement cible n'utilise pas la dernière version de la JVM, les JConsole et VisualVM resteront parfois inopérants et il faudra se retrousser les manches pour aller chercher ces informations en mode *terminal*. Dans ce cas, le document "[Troubleshooting Guide for Java](http://java.sun.com/javase/6/webnotes/trouble/TSG-VM/html/index.html)" constitue la référence indispensable pour générer et exploiter ces *dumps*.

*Article publié sur [LinuxFr](http://linuxfr.org/~galaux/) dans le cadre de mon activité professionnelle à [Linagora](http://linagora.com/).*

