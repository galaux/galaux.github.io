---
layout: post
title: Revue d'architecture pour GWT
description: ""
category: articles
tags: [java,development]
---

*This article in french belongs to a serie of articles I intend to write about a web application I am currently developping for training purpose. The english version of the articles should come up later. In the meantime you can still have a look at [the source code of the application](https://github.com/galaux/diosmio).*

[GWT](https://developers.google.com/web-toolkit/) est un ensemble de composants graphiques pour application web mais qui veut s'y initier s'aperçoit vite que le vrai challenge ne se situe pas dans les composants eux-même mais dans l'architecture de l'application qui utilise ces composants. À la lecture des [tutoriels officiel de Google](https://developers.google.com/web-toolkit/doc/latest/DevGuide), on découvre qu'il existe une multitude de *patterns*, de méthodes et de frameworks pour architecturer une application GWT. Certains sont officiels Google, d'autres non, certains sont compatibles entre eux, certains plus adapté à certains cas d'utilisation.

De plus, beaucoup d'articles quoique très utiles à titre d'exemple proposent des implémentations très minimalistes. Il est rare de voir détaillée une application GWT complète de la couche de persistance à la couche présentation.

Cet article a pour but de survoler à haut niveau ces frameworks afin d'orienter lors d'une initiation à GWT. Le but n'est sûrement pas d'expliquer comment les mettre en oeuvre ; cela ne servirait à rien : internet regorge d'exemples. Enfin un autre article sera consacré à quelques points spécifiques à la mise en place d'un ensemble de ces solutions.

### Model View Presenter (MVP)

[MVP](https://developers.google.com/web-toolkit/articles/mvp-architecture) est le design pattern poussé par Google comme ossature générale d'une application GWT. Il est relativement similaire au [pattern Model View Controller (MVC)](http://fr.wikipedia.org/wiki/Mod%C3%A8le-Vue-Contr%C3%B4leur) : les `Views` déclarent les composants GWT, leurs positionnements, certaines configurations de style, de visibilité puis sont vite rattachées à leur `Presenter` respectif. Celui-ci reçoit les événements des `Views` et les traite en appelant au besoin une couche de service de façon asynchrone. Une fois reçus, le résultat des requêtes serveur est traité par les `Presenters` et affiché dans les `Views`. La déclaration de tous ces éléments est dévolue à un `Controller` qui porte aussi un `EventBus` pour faciliter la communication entre `Presenters`.

La meilleure manière de débuter avec GWT et MVP est de dérouler [le tutoriel de Google en 2 parties](https://developers.google.com/web-toolkit/articles/mvp-architecture). À noter : le framework non-officiel [gwt-platform](http://code.google.com/p/gwt-platform/) permet de faciliter la mise en place de MVP.

### UIBinder

La deuxième partie du [tutoriel de Google](https://developers.google.com/web-toolkit/articles/mvp-architecture) cité précédemment propose l'utilisation de [UIBinder](https://developers.google.com/web-toolkit/articles/mvp-architecture-2). Ce framework permet de déclarer tous les composants des `Views` et leurs configuration initiale dans des fichiers XML et allège donc considérablement leur code. Il permet enfin de disposer d'un éditeur [WYSIWYG](http://fr.wikipedia.org/wiki/What_you_see_is_what_you_get) sous Eclipse. UIBinder est à mon sens un minimum vital pour un projet efficace.

### Appels asynchrones au serveur via XML-RPC

Toujours dans son [tutoriel en 2 étapes](https://developers.google.com/web-toolkit/articles/mvp-architecture), Google met en place une communication client (navigateur)/serveur (webapp) basé sur des appels asynchrones XML-RPC. C'est le système standard qui conviendra à beaucoup d'applications mais qui peut induire des complications lors de l'utilisation de couches de persistance comme [JPA](http://fr.wikipedia.org/wiki/Java_Persistence_API). La norme JPA préconise en effet d'améliorer (*enhance*) le code des entités ce qui les rendra *incompréhensibles* par GWT. En effet pour passer les entités entre le client et le serveur, GWT a besoin de *parser* le code entités afin de les rendre sérialisables. Or ce code source initial des entités que GWT *parse* ne correspond plus au bytecode généré par la compilation puisqu'entre temps JPA est passé par là pour *améliorer* celles-ci. Un article traitera de la mise en place de JPA dans une application GWT dans lequel on verra que la seule solution avec cette technique est de créer des DTO pour GWT.

### Editor

Le [framework Editor](https://developers.google.com/web-toolkit/doc/latest/DevGuideUiEditors) peut être utilisé avec n'importe quel autre framework cité précédemment. Il permet de créer un lien automatique entre les objets traités par la partie serveur ([POJO](http://fr.wikipedia.org/wiki/Plain_Old_Java_Object) `Artifact` par exemple) et les `Views` qui affichent et modifient des instances héritées de `Editor` (`ArtifactEditor`). Un objet hérité de `Driver` fait ensuite le lien entre l'entité et son `Editor`. Le développeur peut ensuite appeler la méthode `driver.edit(artifactEditor)` pour spécifier que les données ont été modifiées dans la vue (charge au développeur de traiter ces changement comme par exemple les répercuter dans le modèle). Attention cependant les vues et leurs éditeurs peuvent devenir plus compliqués à implémenter s'il faut *mapper* un modèle dont les données sont elles-mêmes imbriquées.

### Activity and Place

Comme les composants GWT s'affichent dans un navigateur, l'utilisateur peut être tenté d'utiliser le bouton *Précédent* pour revenir intuitivement à un écran précédent. Cela pose généralement des problèmes aux applications web qui doivent maintenir des informations en session. Le framework [Activity and Places](https://developers.google.com/web-toolkit/doc/latest/DevGuideMvpActivitiesAndPlaces) propose la gestion de cet historique dans l'application. Notons que [le tutoriel de Google sur MVP de Google](https://developers.google.com/web-toolkit/articles/mvp-architecture-2) propose déjà une gestion simple de l'historique via la classe `EventBus`.

### RequestFactory

Le framework [RequestFactory](https://developers.google.com/web-toolkit/doc/latest/DevGuideRequestFactory) proposé par Google remplace dans son emploi les appels XML-RPC entre le client et le serveur. Il permet une réduction du volume d'échanges client/serveur ainsi qu'une proximité plus grande avec la couche de persistance via la déclaration pour chaque entitée (`Artifact` par exemple) d'un `Proxy` correspondant (`ArtifactProxy`). Ce `Proxy` est une version légère de l'entité qui sera utilisable dans la partie cliente de GWT. Le framework RequestFactory se charge de maintenir à jour le `Proxy` avec les éléments de la vue. Là encore, il est possible depuis le client d'appeler le serveur de manière asynchrone.

Cette solution apporte une solution simple au problème de *enhancement* décrit plus haut grâce aux `Proxies` qui sont en quelque sorte des [DTO](http://fr.wikipedia.org/wiki/Objet_de_transfert_de_donn%C3%A9es) mais dont la gestion est dévolue au framework RequestFactory et non plus au développeur. [Le tutoriel proposé par Google](https://developers.google.com/web-toolkit/doc/latest/DevGuideRequestFactory#locators) pousse le concept plus loin en proposant d'utiliser des `Locator`s et `ServiceLocator`s pour lier automatiquement la partie serveur de GWT au DAO de l'application et même de *détecter* et d'utiliser automatiquement ses méthodes standards du type `find()`, `persist()`, `delete()`, etc.

Cette solution semble particulièrement adaptée pour des applications à faible contenu métier, des applications [CRUD](http://fr.wikipedia.org/wiki/CRUD). On peut cependant imaginer utiliser un mélange des deux solutions : RequestFactory pour les parties de l'application qui ne font que créer, supprimer mettre à jour des entités et des DTO pour les parties au métier plus complexe.

L'application complète de test [Dios Mio](http://www.alaux.net/index.php?article34/dios-mio-ecorche-d-une-webapp-fr) dont vous pouvez retrouver [le code source sur Github](https://github.com/galaux/diosmio) met en pratique certains de ces frameworks. Enfin, un prochain article exposera l'intégration de beans [Springs](http://www.springsource.org/) dans Dios Mio.

