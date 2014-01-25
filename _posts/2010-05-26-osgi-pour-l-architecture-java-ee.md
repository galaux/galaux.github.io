---
layout: post
title: OSGi pour l'architecture Java EE
description: "Présentation de l'architecture OSGi"
category: articles
tags: [java,development,server]
---

![OSGi]({{ site.url }}/images/2010/05/OSGi.png)

Bien qu'ayant de nombreux avantages, Java EE génère souvent des applications ou *frameworks* monolithiques, aux tailles conséquentes et donc peu flexibles. Plusieurs sociétés très impliquées dans le développement logiciel se sont donc posé la question de classifier, trier, organiser et homogénéiser les tâches que l'on retrouve régulièrement dans les WARs et EARs. C'est le but du standard [Open Services Gateway initiative](http://www.osgi.org/) (OSGi), qui définit un modèle de gestion de cycle de vie d’une application, un répertoire (registry) de services, un environnement d'exécution et des modules.

Pour combattre la horde insoumise de fonctionnalités récurrentes des applications Java EE, il fallait bien une Alliance : l'[OSGI Alliance](http://www.osgi.org/Main/HomePage) ! Celle-ci a été créée en 1999 par [des entreprises](http://www.osgi.org/About/Members) conscientes dès lors de ce besoin d'harmonisation. On y retrouve les grands noms habituels de l'industrie informatique mondiale tels qu'Oracle, IBM, Red Hat, feu SUN, etc. Le résultat de leurs travaux est la spécification du standard ouvert OSGi [aujourd'hui dans sa version 4](http://www.osgi.org/Specifications/HomePage).

Un des premiers chantier de l'OSGi a été d'identifier les services habituels des applications et *frameworks* Java EE :

- Journalisation
- Configuration, administration
- Accès aux périphériques
- Authentification
- Connections d'entrée/sortie
- Préférences
- ...

Voir [la liste exhaustive sur Wikipedia](http://en.wikipedia.org/wiki/OSGi#Services) ainsi que le graphique suivant qui décrit l'architecture. ![osgi31644\_archi]({{ site.url }}/images/2010/05/osgi31644_archi.gif)

Le standard OSGi préconise ainsi à tout projet Java souhaitant implémenter la norme de découper son code selon ces catégories et d'utiliser un sous-format de *bundle*, un JAR contenant les informations de nom, versions et autres dans un fichier MANIFEST spécifique. Cette proposition d'agencement apporte une grande modularité entre les différents services qui pourront s'appeler entre eux ou proposer leurs services. La norme OSGi spécifie aussi tout un cycle de vie des *bundles* qui doivent pouvoir être installés, démarrés, utilisés, arrêtés, redémarrés et désinstallés automatiquement et à chaud comme le schématise le diagramme ci-contre. Ceci implique la possibilité de mettre à jour et de redémarrer une partie de l'architecture sans impact lourd pour la plate-forme ; une fonctionnalité assez rare en Java. ![250px-OSGi\_Bundle\_Life-Cycle.svg]({{ site.url }}/images/2010/05/250px-OSGi_Bundle_Life-Cycle.svg.png)

Les projets qui tombent dans le giron de cette norme sont très divers : on y retrouve pêle-mêle les serveurs d'applications [GlassFish](https://glassfish.dev.java.net/), [JBoss](http://www.jboss.org/), [JOnAS](http://wiki.jonas.ow2.org/xwiki/bin/view/Main/WebHome) et [Weblogic](http://www.oracle.com/us/products/middleware/application-server/index.htm), des *frameworks* comme [EasyBeans](http://wiki.easybeans.org/xwiki/bin/view/Main/WebHome) ainsi que les IDE libres leaders du marché [NetBeans](http://netbeans.org/) et [Eclipse.](http://www.eclipse.org/) Ce dernier est d'ailleurs un très bon exemple OSGi puisque depuis sa version 3, son coeur ([Equinox](http://www.eclipse.org/equinox/)) est une pure implémentation de la norme OSGi dans sa dernière version.

La concertation des acteurs de l'industrie Java sur des normes telle que l'OSGi ainsi que l'adoption de celle-ci par de grand projets est le garant d'une interopérabilité aujourd'hui indispensable dans le puzzle des *frameworks* Java. On ne peut donc que saluer et encourager ce genre de travaux qui permettent de clarifier la conception d'applications.

*Article publié sur [LinuxFr](http://linuxfr.org/~galaux/) dans le cadre de mon activité professionnelle à [Linagora](http://linagora.com/).*
