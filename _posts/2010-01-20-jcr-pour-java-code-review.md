---
layout: post
title: JCR pour Java Code Review
description: "Comment faire un code review Java avec JCR"
category: articles
tags: [java,tests,development]
---

La revue de code n'est pas une discipline du développement qui évolue sous les feux de la rampe. Souvent sacrifiée sur l'autel de la précipitation, oubliée dans l'ombre d'actions considérées comme plus prioritaires, la revue de code reste cependant une bonne pratique pour maîtriser la qualité des développements au sein de son/ses équipe(s). [Java Code Review](http://jcodereview.sourceforge.net/) propose ses services simples mais efficaces : présenter les derniers *commits*, surligner les modifications, compter le nombre de relectures des modifications etc.

JCR est codé en Python et utilise une base de données pour stocker les différents éléments des projets en cours de revue. L'architecture technique de JCR repose sur le concept de l'application web consultable par les différents acteurs d'un projet de revue de code.

Côté utilisateur, JCR propose différents rôles:
- Un administrateur
- Des *project owners*
- Des *reviewers*

La tâche de gestion des accès aux dépôts SVN est dévolue à l'administrateur, qui a aussi la responsabilité de créer les projets proprement dits. [![01]({{ site.url }}/images/2009/12/01.png)]({{ site.url }}/images/2009/12/01.png)

Une fois cette formalité effectuée, JCR peut accéder aux modifications remontées par les développeurs. Viennent ensuite les *project owners* qui sont généralement les personnes à l'origine du code : ils soumettent leur travail à l'avis des *reviewers*.

Ce découpage en rôles est le garant d'une utilisation aisée à grande échelle même s'il peut paraître compliqué dans un premier temps.

Une fois ces paramétrages effectués et les rôles attribués, JCR permet de générer la liste des fichiers modifiés et la présente sous forme de différentiels colorés, ce qui assure une clarté lors de la revue de code. À chaque lecture d'un fichier modifié, un compteur s'incrémente et permet de s'assurer qu'un nombre suffisant de personnes a revu la modification. Les reviewers et les project owners ont aussi la possibilité d'échanger via un système de notes portant sur une modification du code. [![02]({{ site.url }}/images/2009/12/02.png)]({{ site.url }}/images/2009/12/02.png)

Pour l'instant JCR ne supporte qu'[un seul gestionnaire de version](http://jcodereview.sourceforge.net/html/faq.html#why-jcr-instead-of-other-code-review-tools) (SVN), même si l'intégration avec d'autres gestionnaires est prévue. L'interface de JCR peut ensuite se révéler peu intuitive : créer son premier projet s'avère parfois compliqué pour le débutant du fait de l'arrangement des liens. Enfin, la documentation a tendance à être verbeuse et à perdre l'utilisateur curieux.

Le déploiement est simple et efficace : une version *standalone* pour les plus pressés permet de goûter au produit et de jouer avec ses fonctionnalités. Une fois convaincu, une installation complète permet de passer le jouet en production et de soumettre ses propres projets sans trop d'efforts. Remarquons que JCR supporte pour cela plusieurs SGBD comme SQLite (très utile pour la version *standalone*), PostgreSQL et MySQL.

Java Code Review est simple dans sa conception et il propose des outils efficaces de revue de code. Même si la configuration de ses premiers projets peut s'avérer déroutante, JCR reste intéressant et devrait trouver plus souvent sa place dans les environnements de développement au bénéfice de la qualité des projets informatiques.

*Article publié sur [LinuxFr](http://linuxfr.org/~galaux/) dans le cadre de mon activité professionnelle à [Linagora](http://linagora.com/).*

