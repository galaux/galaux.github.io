---
layout: post
title: XWiki - présentation
description: "Découverte du Wiki collaboratif et bien plus"
category: articles
tags: [java]
---

[XWiki](http://www.xwiki.org/xwiki/bin/view/Main/WebHome) est une solution open source de Wiki proposée par la société du même nom. Développé depuis 2003 et maintenant sous licence LGPL, XWiki est un produit assez complet et visuellement réussi. Même si ce type de solution est très courante sur le net ou dans les intranet de sociétés, attention à ne pas le confondre avec le premier venu. Il se targue en effet d'être non seulement un "Wiki d'entreprise" mais aussi un "Wiki Applicatif" ce qui fait de lui bien plus qu'un simple outil de gestion d'articles.

### Un wiki d'entreprise
Du point de vue utilisateur, XWiki propose les fonctionnalités de base de tout bon wiki à commencer par la gestion *riche* de textes via un éditeur [*WYSIWYG*](http://fr.wikipedia.org/wiki/WYSIWYG) et la prise en compte de l'historique des modifications. Mais XWiki commence déjà à poser sa marque : un soin particulier a été pris sur la gestion des pièce-jointes ce qui fait parfois défaut à certains de ses concurrents. Ses utilisateurs auront en plus la possibilité de personnaliser les barres latérales de leur interface pour améliorer leur appréhension personnelle de l'outil. Les administrateurs ne seront pas en reste puisqu'ils pourront gérer les utilisateurs via LDAP et les affecter à des groupes et jouer sur leurs droits. Il faut aussi noter la présence de tableaux de bord présentant la liste des dernières modifications et d'indicateurs de statistiques des visites. Cerise sur le gâteau, XWiki peut vous prévenir des modifications de contenu par mail ou via flux RSS.

[![](http://www.08000linux.com/blogs/files/2009/10/wikipeople.png)](http://www.08000linux.com/blogs/files/2009/10/wikipeople.png)

### Un wiki applicatif
XWiki propose une facette plus rare chez ses camarades : la possibilité de le modeler pour qu'il réponde à vos besoins en applications d'entreprise. On peut ainsi utiliser [XWiki comme un blog](http://www.xwiki.com/xwiki/bin/view/BlogFr/?language=fr) mais aussi comme un [gestionnaire de tâches attribuées à vos utilisateurs](http://www.xwiki.com/xwiki/bin/view/Products/XWikiEnterprise?tab=header-tab2#header-tab2). On peut suivre le degré de complétion de ces tâches mais aussi les flux d'activités publiés sur les travaux des membres de votre équipe. Ceci fait pleinement passer XWiki dans le monde des portails d'entreprise. Non content de proposer un univers riche dans ce domaine, XWiki permet de rattacher des [composants](http://platform.xwiki.org/xwiki/bin/view/Features/) supplémentaires ou d'[en écrire d'autres](http://code.xwiki.org/xwiki/bin/view/Modules/ComponentModule). L'utilisation du langage [Groovy](http://en.wikipedia.org/wiki/Groovy_%28programming_language%29) ou du système de template de [Velocity](http://en.wikipedia.org/wiki/Jakarta_Velocity) font partie des exemples les plus coura

### Le plan technique
Sur le plan technique, XWiki est codé en Java et repose sur plusieurs briques logicielles bien connues comme Hibernate pour l'accès aux données et Lucene pour l'indexation des pages. Le moteur de rendu des pages est pris en charge par un [moteur](http://code.xwiki.org/xwiki/bin/view/Modules/RenderingModule) spécialement codé pour l'occasion. XWiki [supporte plusieurs types de SGBD](http://platform.xwiki.org/xwiki/bin/view/AdminGuide/Database+Administration) dont DB2, MySQL et PostgreSQL.

Intranet, site internet, blog, application de gestion de tâche, de communication… XWiki est donc une solution très étendue et puissante qui ne se contente pas de l'image simpliste du gestionnaire d'article qui colle au wiki. Avec son principe de composants, la limite de XWiki n'est réellement plus que dans l'imagination de ses utilisateurs.

*Article publié sur [LinuxFr](http://linuxfr.org/~galaux/) dans le cadre de mon activité professionnelle à [Linagora](http://linagora.com/).*
