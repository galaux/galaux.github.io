---
layout: post
title: Neo4j, un SGBD en graphe pour Java
description: "La base de donnée en graphe Neo4j"
category: articles
tags: [development,database,java]
---

Le NoSQL, ce mouvement qui plaide pour un relâchement de certaines règles sur le stockage des données, a le vent en poupe. Son nom évocateur se veut en opposition avec les principes de stockage en base de données SQL, très répandu et régis par des règles de stockage très strictes. Il est le sujet de beaucoup d'articles sur internet et on le retrouve dans les projets de géants du web comme [Facebook](http://cassandra.apache.org/) et [Twitter](http://github.com/traviscrawford/scribe). Dans la lignée du NoSQL, [Neo4j](http://neo4j.org/) propose l'idée originale de stocker des données sous la forme d'un graphe.

Fini donc les tables et les colonnes des bases de données relationnelles: Neo4j est une SGBD embarqué pour Java qui se repose sur des graphes. Comprendre par là des [graphes mathématiques](http://fr.wikipedia.org/wiki/Th%C3%A9orie_des_graphes) faits de noeuds, de relations nommées entre ces nœuds, à l'exception que les nœuds deviennent des objets de la vie réelle avec leurs propriétés. Les relations entre ces objets sont conservées et peuvent de fait porter un nom. Dans l'exemple suivant, les développeurs de Neo4j recrééent le réseau des personnages du film Matrix :

{% highlight java %}
NeoService neo = ... // Utilisation d'une factory
// Création de Thomas Anderson
Node mrAnderson = neo.createNode();
mrAnderson.setProperty( "name", "Thomas Anderson" );
mrAnderson.setProperty( "age", 29 );

// Création de Morpheus
Node morpheus = neo.createNode();
morpheus.setProperty( "name", "Morpheus" );
morpheus.setProperty( "rank", "Captain" );
morpheus.setProperty( "occupation", "Total bad ass" );

// Création de la relation
// afin de représenter le lien entre ces personnages
mrAnderson.createRelationshipTo( morpheus, RelTypes.KNOWS );
// Traduction : "mrAnderson KNOWS morpheus"
{% endhighlight %}

On retrouve cette relation, ainsi que d'autres, dans le graphe suivant : [![graphe](http://08000linux.com/blogs/files/2010/05/graphe.png)](http://08000linux.com/blogs/files/2010/05/graphe.png)

L'utilisation des données que le code précédent a créé est assez triviale ; par exemple pour retrouver les amis de "Mr Anderson" :

{% highlight java %}
// Instantiation d'un mécanisme de parcours (traverser)
// qui retourne les amis de mrAnderson
Traverser friendsTraverser = mrAnderson.traverse(
  // recherche en largeur d'abord
  Traverser.Order.BREADTH_FIRST,
  StopEvaluator.END_OF_NETWORK,
  // ne pas retrourner mrAnderson
  ReturnableEvaluator.ALL_BUT_START_NODE,
  RelTypes.KNOWS,
  Direction.OUTGOING
);

// Utilisation de ce mécanisme de parcours
// pour afficher le résultat
System.out.println( "Mr Anderson's friends:" );
for ( Node friend : friendsTraverser ) {
  System.out.printf( "At depth %d => %s%n",
    friendsTraverser.currentPosition().getDepth(),
    friend.getProperty( "name" )
  );
}
{% endhighlight %}

L'originalité de cette conception en graphe est de stocker les objets et leurs relations tels qu'ils existent dans les cas réels.

Le projet Neo4j n'est pas nouveau : depuis 10 ans déjà, cette implémentation de la base de données en graphes propose ses services et s'enorgueillit d'être utilisé en environnements de production depuis 7 ans. Les capacités annoncées n'ont pas à pâlir devant un SGBD relationnel : ses développeurs annoncent de bonnes qualités de scalabilité et de performances ainsi que la capacité à stocker jusqu'à plusieurs milliards de noeuds!

C'est un point de vue original sur le stockage de données que proposent les SGBD objets comme Neo4j. À l'heure où les SGBD *leaders* du marché se basent tous sur le modèle relationnel, voilà une solution qui prend le contrepied de la tendance. Un grand bravo donc à ces projet originaux comme [db4o](http://www.db4o.com/), [Zope Object Database](http://www.zodb.org/) et Neo4J qui semble bien détenir une réponse efficace au problème du stockage de données objets.

*Article publié sur [LinuxFr](http://linuxfr.org/~galaux/) dans le cadre de mon activité professionnelle à [Linagora](http://linagora.com/).*
