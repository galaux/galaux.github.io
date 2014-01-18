---
layout: post
title: Le point sur Java 7 (1/2)
description: ""
category: articles
tags: [java,development]
---

![Wave](http://08000linux.com/blogs/files/2010/02/Wave.png)

Pour les développeurs Java, les nouveautés que réserve SUN pour les prochaines versions de son langage Orienté Objet sont autant de sujets d'attentes, de débats, et même pour certains ... d'espoirs ! Beaucoup a été dit et surtout *bloggué* depuis le lancement du projet Java 7 "Dolphin", les échanges ont été nombreux et le débat fourni. À l'heure où la sortie de cette dernière mouture de Java est imminente, faisons le point sur le sujet et tout particulièrement sur le projet [Coin](http://openjdk.java.net/projects/coin/) de SUN et des améliorations qu'il apporte à Java 7. Il sera suivi d'un autre article sur les modifications lancées en parallèle de ce projet pour Java 7.

Plusieurs chantiers majeurs d'évolutions ont donc été lancés en interne autour d'OpenJDK. Mais SUN a aussi voulu intégrer la communauté Java dans le projet pour prendre en compte le retour d'expérience des utilisateurs quotidiens de sa plate-forme. À cet effet, SUN a initié le projet [Coin](http://openjdk.java.net/projects/coin/) dans le but de lister les "petites" améliorations qui pourraient être intégrées. SUN a donc misé sur le débat ouvert puisqu'il était permis à tous de soumettre ses idées d'évolution. Et le moins que l'on puisse dire est que ce "projet autour du projet" a porté ses fruits puisque ce n'est pas moins de 70 propositions qui ont été soumises (voir cet [article du blog d'Alex Miller](http://tech.puredanger.com/java7) qui en a fait une liste exhaustive et documentée). S'en est suivie une longue période de débats, mais après quelques rebondissements de dernière minutes, nous connaissons (enfin) les 5 propositions de la communauté qui ont été retenues :

**Possibilité d'utiliser des *String* dans les *switch***\
 Tout développeur Java s'est déjà retrouvé dans l'impossibilité d'utiliser des chaînes de caractères dans les structures switch. Cette fonctionnalité bien pratique existait depuis longtemps dans bien des langages et ce sera bientôt chose faite pour Java.

**Gestion automatique des ressources**\
 La mémoire est gérée automatiquement en Java et cela dans le but de soulager le développeur dans le développement d'applications haut niveau. Cependant cela n'était pas le cas des autres ressources comme les documents qu'il fallait ouvrir, et surtout, fermer à la main. Cette amélioration de Java 7 vise à automatiser cette gestion même s'il est peu probable que l'on en arrive au niveau de gestion de la mémoire.

**Amélioration des instanciations génériques**\
 Voici une nouveauté qui ne sera en fait qu'une simplification d'écriture dans le but d'alléger le code. Il ne sera désormais plus nécessaire de spécifier le type des éléments de listes génériques lors de la déclaration ET de l'allocation, celle-ci devenant évidente.\
 Voici un exemple avec la syntaxe actuelle :\
 List\<Integer, List\<String\>\> liste = new ArrayList\<Integer, List\<String\>\>();\
 Java 7 permettra d'écrire :\
 List\<Integer, List\<String\>\> liste = new ArrayList\<\>();

**Simplification des *varargs***\
 Voici une autre modification qui a pour but de simplifier la vie du développeur sans pour autant apporter de grande nouveauté. En l'occurrence l'emploi du code suivant qui était jusqu'alors sujet au warning "uses unchecked or unsafe operations". \
 `static <T> List<T> asList(T... elements) { ... } static List<Callable<String>> stringFactories() {  Callable<String> a, b, c;  ...  *// Warning: **"uses unchecked or unsafe operations"*  return asList(a, b, c); }`\

**Support de langages de script**\
 Un des sujets d'attentes était la possibilité d'intégrer des langages de script comme le Ruby et le Python dans la JVM. C'est désormais le cas via la JSR 292 (Java Specification Request). Ceci est expliqué plus en détail dans ce [mail d'archive](http://mail.openjdk.java.net/pipermail/coin-dev/2009-March/001131.html) de la mailing list du projet Coin.

***Closures*** ou non?\
 Comme expliqué plus haut dans l'article, des évènements de dernières minutes sont venus créer des remous dans la communauté Java. Beaucoup de membres espéraient en effet que la proposition d'intégration des [Closure](http://fr.wikipedia.org/wiki/Fermeture_(informatique))allait être acceptée. Cependant celles-ci ne figuraient pas dans la liste des améliorations retenues par le projet Coin pour Java 7. Rebondissement lors de la conférence Devoxx 2009 : Mark Reinhold, ingénieur chez SUN sur le projet OpenJDK annonçait finalement [leur acceptation](http://www.jroller.com/scolebourne/entry/more_detail_on_closures_in). Depuis aucune information n'est venue confirmer ou infirmer ce dernier commentaire. Pour mémoire, les closures, qui existent déjà dans nombre de langages comme le C++, sont des sous-fonctions définies dans le corps de fonctions et qui portent sur les variables locales de cette fonction.

Comme on peut le voir, les nouveautés apportées par le projet Coin pour Java 7 sont principalement d'ordre esthétique mais ce sont aussi ces détails qui facilitent le quotidien du développeur. À noter que nombre de propositions plus "de fond" n'ont pas été retenue comme par exemple l'autorisation des multi-catch pour gérer plusieurs types d'exceptions en une fois.

**À suivre : les chantiers internes de SUN pour Java 7.**

*Article publié sur [LinuxFr](http://linuxfr.org/~galaux/) dans le cadre de mon activité professionnelle à [Linagora](http://linagora.com/).*

