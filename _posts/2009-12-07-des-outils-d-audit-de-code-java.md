---
layout: post
title: Des outils d'audit de code Java
description: "Les solutions pour auditer du Java"
category: articles
tags: [java,tests,development]
---

Le langage Java n'est pas un langage qui s'exprime simplement avec un éditeur de texte et un compilateur... Les IDE et outils d'aide au développement sont légion et pour cause, il serait dangereux de s'en passer. C'est particulièrement vrai pour les outils d'audit de code dont voici deux exemples.

![UCDetector\_popup]({{ site.url }}/images/2009/11/UCDetector_popup.png)

[UCDetector](http://www.ucdetector.org/) (prononcer "You See Detector") est un plugin Eclipse qui comme son nom l'indique se propose de détecter le code mort (Un-needed Code Detector) dans les sources Java.

Premier avantage : son format de plugin Eclipse en fait un outil très rapide à déployer. Sous Eclipse, ajoutez le site http://ucdetector.sourceforge.net/update dans le menu "Aide \> Installer de nouveaux logiciels" et vous n'êtes plus qu'à trois clics d'avoir un outil d'audit de code fonctionnel. UCDetector propose quelques options via le menu "Fenêtre \> Préférences \> UCDetector" mais il n'y a pas de quoi s'y perdre.

![UCDetector\_popup]({{ site.url }}/images/2009/11/UCDetector_quickfix.png)

La possibilité d'ignorer certains fichiers, méthodes, champs, de restreindre les résultats ou de les paramétrer est probablement la plus utilisée. Outre l'affichage dans Eclipse, l'export aux formats XML et HTML restent des fonctionnalités standard mais appréciables. La pertinence des rapports générés est bonne pour peu que l'on sache faire le tri dans la liste des remarques générées. Pour affiner les résultats ou décupler les possibilités de l'outil il sera ensuite nécessaire de mettre les mains dans le cambouis pour [créer soi-même ses marqueurs](http://www.ucdetector.org/custom.html) en fonction de ses besoins.

[PMD]({{ site.url }}/images/2009/11/PMD.png) est un concurrent possible d'UCDetector mais cette fois-ci l'installation n'est pas triviale et pour cause : PMD est une application complète en ligne de commande. Même si beaucoup de plugins sont disponibles et rendront son utilisation quotidienne aisée sous Eclipse, NetBeans etc., la configuration de ses règles via des fichiers XML semble infinie et nécessitera donc un peu d'étude... Mais la puissance du logiciel lorsqu'il est bien configuré mérite très largement ces efforts. Pour les vérifications de base, certaines configurations initiales sont proposées : "basic, imports, unusedcode" mais on comprend vite que la puissance de l'outil se trouve dans la personnalisation de ses règles pour les besoins particuliers de tel ou tel projet. PMC propose aussi d'exporter vers des fichiers et peut même être intégré à des outils de build comme Maven et Ant.

[![PMD]({{ site.url }}/images/2009/11/PMD.png)]({{ site.url }}/images/2009/11/PMD.png)

Même s'ils proposent les mêmes services, UCDetector et PMC n'intéresseront pas les mêmes populations. UCDetector conviendra à un développeur souhaitant un filet de protection relativement simple mais toujours pertinent. Son intégration dans Eclipse est très bonne et rapide. Le fait qu'il y ait peu de configuration rend l'outil très "plug'n play" ce qui est utile quand on souhaite le mettre en place au milieu d'une phase de développement. PMD quant à lui est plus long à installer et à configurer mais il fait vite sentir la puissance qui en résulte. Il n'est pas le genre d'outil qu'on installe sans une étude préalable de sa configuration au risque de se retrouver avec des rapports peu exploitables. PMD se positionne plus comme l'outil à mettre en place automatiquement sur un ensemble de projets par l'architecte à l'attention de ses développeurs. Et si l'outil est effectivement puissant, il faut mieux ne pas l'utiliser que de perdre les développeurs sous une avalanche de messages peu pertinents voire inexploitables et incompréhensibles !

*Article publié sur [LinuxFr](http://linuxfr.org/~galaux/) dans le cadre de mon activité professionnelle à [Linagora](http://linagora.com/).*

