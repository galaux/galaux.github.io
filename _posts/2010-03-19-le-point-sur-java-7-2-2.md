---
layout: post
title: Le point sur Java 7 (2/2)
description: "Les nouveautés qu'apportent Java 7 (2/2)"
category: articles
tags: [java,development]
---

![Duke]({{ site.url }}/images/2010/02/duke.png)

SUN s'apprête à proposer sa nouvelle version de Java 7 et pour ce faire, deux types de réflexions ont été lancées. La première s'appuyait sur le retour de la communauté et a déjà été abordée dans un article précédent. L'autre est interne à SUN et porte sur [des modifications plus profondes du framework Java](http://openjdk.java.net/projects/jdk7/features/).

SUN développe des efforts en direction des performances de la JVM comme en témoigne ce [projet de compression des adresses des pointeurs 64 bits](http://wikis.sun.com/display/HotSpotInternals/CompressedOops). Mais une amélioration plus intéressante encore se situe au niveau du *Garbage Collector*, l'outil de libération mémoire des objets du développeur. Le nouveau *Garbage Collector* "[Garbage First G1](http://tech.puredanger.com/2008/05/09/javaone-g1-garbage-collector)" devrait passer moins de temps en pause et ainsi d'améliorer l'efficacité de la JVM. Espérons que ces promesses se réaliseront : le *Garbage Collector* joue un rôle sensible et est souvent un acteur majeur dans nombre de cas d'applications aux performances déteriorées. Dans la veine de ces améliorations, citons aussi le *classloader* et la concurrence dans les accès IO qui devraient être revus ainsi qu'une bonne partie du chapitre 2D des applications client léger Java.

Autre grande nouveauté du langage Java à proprement parler : la possibilité d'[annoter les types](http://openjdk.java.net/projects/type-annotations/) afin de leur adjoindre certaines caractéristiques. L'auteur de la [JSR en cause](http://jcp.org/en/jsr/detail?id=308) propose des exemples très parlants dont la seule vue devrait inspirer les développeurs Java tant ils produisent du code simple et clair :

{% highlight java %}
public int size() @Readonly { ... } Map<@NonNull String, @NonEmpty List> files;
// Au sujet des tableaux :
Document[@Readonly] docs1; Document[][@Readonly] docs2 = new Document[2][@Readonly 12];
// *Cast* de type :
myString = (@NonNull String)myObject;
// Tests de types :
boolean isNonNull = myString instanceof @NonNull String;
// Création d'objet :
new @NonEmpty @Readonly List(myNonEmptyStringSet)
// Clauses *throws*
void monitorTemperature() throws @Critical TemperatureException { ... }
{% endhighlight %}

Dernière amélioration majeure qui va sûrement retenir l'attention des utilisateurs de Java : la modularisation de la JVM. Le [projet Jigsaw](http://openjdk.java.net/projects/jigsaw/) attendait depuis longtemps dans les cartons de SUN mais n'avait pas réussi à voir le jour tant ses implications sont importantes. La JVM de SUN est devenue très volumineuse et cela se ressent lors de son téléchargement et de son lancement. Ce projet a pour but de modulariser la JVM pour permettre de ne faire télécharger à l'utilisateur que ce dont il a besoin un peu à la manière d'un distribution Linux. En cas d'utilisation d'une nouvelle application, il suffirait alors de compléter la JVM par les modules qui n'ont pas encore été téléchargés la première fois et dont cette nouvelle application a besoin. L'apport en terme de souplesse de lancement est tout aussi prometteur.

Voilà donc de bien belles promesses qui ne demandent plus qu'à être testées dans les [*snapshots* disponibles de Java 7](https://jdk7.dev.java.net/).

*Article publié sur [LinuxFr](http://linuxfr.org/~galaux/) dans le cadre de mon activité professionnelle à [Linagora](http://linagora.com/).*
