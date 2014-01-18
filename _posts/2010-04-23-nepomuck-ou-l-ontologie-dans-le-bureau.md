---
layout: post
title: Nepomuck ou l'ontologie dans le bureau
description: ""
category: articles
tags: [desktop]
---

[![nepomuk-logo.320](http://08000linux.com/blogs/files/2010/04/nepomuk-logo.320.png)NEPOMUK](http://nepomuk.kde.org/) est un outil d'extraction et de stockage de méta informations sous KDE. Son nom est l'acronyme de *Network Environment for Personalized, Ontology-based Management of Unified Knowledge*. L'équipe de développement avait introduit cette nouveauté dans la version 4 de KDE sortie début 2008. Petit tour d'un outil mal connu qui pourtant peut s'avérer très utile pour exploiter des méta données, et donc proposer des développements de nouvelles fonctionnalités aux utilisateurs...

Caché derrière l'acronyme barbare de NEPOMUK, se cache le concept (non-moins "obfusqué") d'[ontologie](http://fr.wikipedia.org/wiki/Ontologie_%28informatique%29). Dans le domaine informatique, l'ontologie touche aux méta-données, ces informations qui ne sont pas stockées dans les fichiers de l'utilisateur mais qui sont plutôt des données descriptives des fichiers ainsi qu'aux liens que l'on peut déduire de ces informations. Ces méta-données qui intéressent NEPOMUK sont par exemple les *tags* des fichiers MP3 : ce n'est pas la donnée primaire du fichier MP3 mais des caractéristiques additionnelles stockées dans le fichier.

Les développeurs du projet NEPOMUK divisent ces informations en 3 catégories : - données du bureau (comme les *tags* des MP3, droits sur les fichiers, *timestamps*, ...) - informations rajoutées par l'utilisateur (nom de l'auteur d'un document OpenOffice, annotations sur un email, ...) - les données qui ne figurent pas dans les deux catégories précédentes (et qui sont beaucoup plus difficile à récupérer : emplacement depuis lequel un fichier a été téléchargé, pièces-jointes des emails qui peuvent perdre leur information de lien avec l'email lors de leur sauvegarde sur le disque dur)

Le but de NEPOMUK est ainsi de proposer des outils pour créer, gérer et requêter sur ces données et ainsi de pouvoir exploiter cette somme d'information dont on ne soupçonne pas toujours la présence.

Au démarrage de KDE 4 on ne peut passer à côté de ce démon qui semblait utiliser beaucoup de ressources pour indexer les fichiers dans les premières heures de KDE 4. Cette gestion semble avoir été revue et il suffit de s'armer de patience lors d'un premier démarrage de l'environnement e bureau pour pouvoir utiliser l'outil. KRunner (un lanceur d'application de KDE que l'on peut appeler via le raccourci Alt+F2) propose par exemple un plugin pour NEPOMUK qui permet ainsi d'accéder à la liste des fichiers du répertoire *home* de l'utilisateur. "Alt+F2", "archli" et voilà tous les fichiers indexés correspondants au thème "ArchLinux" recherché. ![krunner](http://08000linux.com/blogs/files/2010/04/krunner.png)

Derrière le terme barbare d'ontologie se cache pour le domaine de l'environnement de bureau une mine d'information encore non exploitée que l'on atteint enfin avec NEPOMUK. L'avenir nous dira si l'exploitation de ces informations se révèlera pertinente pour l'utilisateur. Pour cela les développeurs d'applications "de bureau" ont maintenant des outils : charge à eux de les exploiter.

*Article publié sur [LinuxFr](http://linuxfr.org/~galaux/) dans le cadre de mon activité professionnelle à [Linagora](http://linagora.com/).*

