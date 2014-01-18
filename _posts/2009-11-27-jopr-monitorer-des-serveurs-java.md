---
layout: post
title: Jopr : monitorer des serveurs Java
description: ""
category: articles
tags: [java,administration]
---

[Jopr](http://www.jboss.org/jopr) (prononcer "djopeur") est une solution Java de surveillance et d'administration de serveur JBoss. Cet outil open source est aussi simple d'utilisation que puissant. On peut se dire qu'il doit y avoir sous ces quatre lettres un produit potentiellement intéressant, quand on sait que son promoteur n'est autre que RedHat.

Pour être exact, précisons qu'un autre outil RedHat se trouve à l'origine de Jopr : RHQ. Il propose des fonctionnalités de surveillance et d'administration sur une vaste gamme d'applications. Jopr est donc la spécialisation de RHQ pour JBoss.\
 Le déploiement de Jopr passe par l'installation d'agents sur les systèmes distants à administrer. Ceux-ci ouvrent l'accès à la configuration ainsi qu'à des statistiques complètes sur le serveur JBoss local. Un serveur prend ensuite en charge la synthèse des informations collectées et la présentation à l'administrateur. Il faut noter la capacité très utile qu'ont les agents à découvrir automatiquement les ressources dont ils ont la responsabilité comme montré dans la capture suivante. [![01\_JON-Dashboard](http://08000linux.com/blogs/files/2009/11/01_JON-Dashboard.png)](http://08000linux.com/blogs/files/2009/11/01_JON-Dashboard.png)

Une fois l'architecture en place, Jopr offre des fonctionnalités d'administration poussées comme par exemple l'accès à la liste des librairies déployées sur les nœuds. Pour exploiter les pleines possibilités de l'outil, il suffira de piocher dans ses plugins qui vont jusqu'à proposer d'administrer le serveur Jopr lui-même (plugin RHQ-Server) :

-   Hibernate
-   JBossAS
-   Tomcat
-   RHQ-Server
-   JBossCache
-   JBossOSGi

Les informations collectées sont présentées de façon claire via des graphiques ce qui devient vite indispensable pour diagnostiquer les erreurs dans les architectures Web complexes. Les seules "exigences" techniques de Jopr portent sur une JVM récente (supérieure ou égale à 1.5) et une base de données PostgreSQL ou Oracle. [![JON-Monitoring](http://08000linux.com/blogs/files/2009/11/JON-Monitoring.png)](http://08000linux.com/blogs/files/2009/11/JON-Monitoring.png)

Pour être complet sur le sujet, citons JBoss Operations Network (JON) qui est la version payante de Jopr. Jopr et JON jouent ainsi la carte du business plan "logiciel gratuit/support payant". Pour de plus amples informations, il suffit de se référer à la documentation utilisateur et/ou développeur de RHQ qui, pour rappel, constitue le cœur de Jopr.

*Article publié sur [LinuxFr](http://linuxfr.org/~galaux/) dans le cadre de mon activité professionnelle à [Linagora](http://linagora.com/).*

