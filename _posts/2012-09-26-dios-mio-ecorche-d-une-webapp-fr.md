---
layout: post
title: Dios Mio, écorché d'une webapp
description: ""
category: articles
tags: [java,development,spring,jpa]
---

*This article in french is the first of a serie I intend to write about a web application I am currently developping for training purpose. The english version of the articles should come up later. In the meantime you can still have a look at [the source code of the application](https://github.com/galaux/diosmio).*

[*Dios Mio*](https://github.com/galaux/diosmio) est une application web Java EE test qui me sert de laboratoire pour mettre en pratique différents frameworks, API, concepts d'architecture. Elle est en perpétuelle évolution/remodelage et n'est donc pas utilisable en l'état. L'application ainsi que cette série d'articles sont publiés dans le but de partager l'expérience acquise lors de la résolution de certains problèmes d'architecture qui n'ont évidemment pas manqué de se poser.

Dios Mio propose la gestion d'entités *Artifact* que l'on peut voir comme le fil conducteur de l'application.

Cet article est une présentation générale de l'application. Il introduira aussi certains points d'architecture sensibles qui seront développés par la suite dans d'autres articles pour constituer une sorte d'écorché d'une application web. Ces articles sont agrémentés de morceaux de code et/ou de liens vers les classes Java complètes. Vous pouvez en effet retrouver [l'intégralité du code source de Dios Mio](https://github.com/galaux/diosmio) sur [Github](https://github.com).

### Architecture générale

Dios Mio est une webapp [GWT](https://developers.google.com/web-toolkit/) [Spring](http://www.springsource.org/). Son coeur est constitué d'une couche service qui à son tour repose sur une couche [DAO](http://fr.wikipedia.org/wiki/Objet_d'acc%C3%A8s_aux_donn%C3%A9es) qui sont toutes les deux des beans Spring.

                                               Webapp
                                     +-----------------------+
                                     | +-------------+ +---+ |
                                     | |     GWT     | |   | |
                                     | +-------------+ | S | |
                                     |        |        | P | |
                     +-------------+ | +-------------+ | R | |
                     |     CLI     |-|-|  Services   | | I | |
                     +-------------+ | +-------------+ | N | |
                                     |        |        | G | |
                                     | +-------------+ |   | |
                                     | |  DAO (JPA)  | |   | |
                                     | +-------------+ +---+ |
                                     +--------|--------------+
                                           -------
                                         ( BD (H2) )
                                           -------

La couche DAO utilisait initialement [Hibernate](http://www.hibernate.org/) et persistait les entités dans une base [Cassandra](http://cassandra.apache.org/). Cependant afin de conserver la possibilité d'utiliser une autre implémentation, Hibernate a été remplacé par du pur [JPA](http://fr.wikipedia.org/wiki/Java_Persistence_API). De même l'implémentation est maintenant [OpenJPA](http://openjpa.apache.org/) et la base de donnée utilisée est [H2](http://www.h2database.com/html/main.html) qui apporte la facilité de ne pas avoir à lancer de base avant de démarrer l'application.

La rôle de partie présentation est tenu par une application web GWT qui est aussi le "point d'entrée" Spring (L'intégration des beans Spring dans la partie GWT fera l'objet d'un article). C'est donc cette application GWT qui via Spring, déclare les beans que sont la couche service et la couche DAO. Ce rôle de partie présentation pourrait être tenu par n'importe quel autre type de webapp grâce à l'utilisation de Spring.

Dios Mio comporte aussi un accès en ligne de commande (CLI). C'est un simple JAR qui propose un mode connecté à la webapp via [RMI](http://fr.wikipedia.org/wiki/RMI_(java)). Ce CLI comporte aussi l'ébauche de communication à distance avec la webapp par l'intermédiaire d'appels [RPC](http://fr.wikipedia.org/wiki/Remote_procedure_call) sérialisés par [Google Protocol Buffers](http://code.google.com/p/protobuf/) et/ou [Apache Thrift](http://thrift.apache.org/). Cette partie n'est pas achevée par manque d'intérêt mais ne nécessiterait pas de gros développement pour disposer d'appels distants à l'application.

Ce CLI propose une invite de commande avec auto-complétion des commandes saisies par l'utilisateur via [jline](http://jline.sourceforge.net/). Celles-ci sont parsées pour vérifier que leur syntaxe est conforme au dialecte attendu grâce à [ANTLR](http://www.antlr.org/). Ceci sera détaillé dans un article à venir.

Dernier point intéressant : `KissLogger`, un logger très simple ([K](http://fr.wikipedia.org/wiki/Principe_KISS)[I](http://fr.wikipedia.org/wiki/Kiss)[SS](http://fr.wikipedia.org/wiki/Principe_KISS)) pour applications Java lourde (côté CLI donc). Il apporte les avantages suivants :

-   ne pas avoir à ajouter de dépendance à un framework de log
-   léger (1 classe)
-   très simple d'emploi
-   permet de logger temporairement dans un buffer lors du lancement de l'appli puis de rediriger le buffer vers une sortie finale. Voir l'article qui y sera consacré.

Le tout est assemblé par [Maven](http://maven.apache.org/) y compris la partie GWT que l'on peut donc lancer via Maven. Un article traitera de la création du projet Maven GWT, de son import sous [Eclipse](http://www.eclipse.org/) ainsi que de la configuration nécessaire pour lancer et debugger l'application sous Maven et sous Eclipse.

Remarque: Dios Mio étant un projet test et non une application à but de production, je ne me suis pas attardé sur les tests unitaires. C'est une pratique que je ne m'autorise pas sur des projets professionnels.

### Les modules Maven

Dios Mio est compilée/assemblée par Maven et développée initialement sous [IntelliJ](http://www.jetbrains.com/idea/) mais à cause de besoins GWT, le développement se poursuit sous Eclipse.

L'application se décompose en quatre modules Maven plus un module web de test :

-   `diosmio-services` :\
     Contient les DAO ainsi que les services de plus haut niveau. Tous sont injectables via Spring.
-   `diosmio-com` :\
     Contient les entités annotées JPA utilisées dans diosmio-services ainsi que les classes de communication externes Thrift et/ou Protobuf (ébauches).
-   `diosmio-gwt` :\
     Contient l'application web GWT et déclare les beans Spring services, DAO et RMI Exporter qu'elle utilise ainsi que tous les autres beans utilisés dans l'application.
-   `diosmio-cli` :\
     Contient le code de la CLI avec son connecteur RMI.
-   `diosmio-web` :\
     Contient l'application web sans JSP dont le but était d'instancier les beans Spring quand la partie GWT n'existait pas encore.

Tout point de vue ou question sur cette application sont les bienvenus. Ce n'est peut-être pas la meilleure implémentation, c'en est juste une parmi d'autres. Si vous pensez qu'une autre solution pour tel ou tel point d'architecture aurait été plus judicieuse, n'hésitez pas à laisser un commentaire. Enfin si vous pensez que vous pourriez avoir besoin de mes compétences dans votre équipe, mon CV se trouve en haut de cette page ;).

