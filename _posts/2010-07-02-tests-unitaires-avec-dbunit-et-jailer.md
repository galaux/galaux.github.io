---
layout: post
title: Tests unitaires avec DBUnit et Jailer
description: "Pousser les tests unitaires Java"
category: articles
tags: [java,tests,development]
---

Dans le domaine du développement, et particulièrement le développement Java, les outils Open Source sont très largement utilisés en entreprise. Quand il s'agit de mettre en place des tests unitaires en Java, le nom de [JUnit](http://www.junit.org/) revient souvent. Même si quelques alternatives ont vu le jour (en particulier [TestNG](http://testng.org/doc/index.html)), ce *framework* reste le *leader* dans son domaine.

Pour ceux qui utilisent déjà JUnit, voici deux outils complémentaires qui en facilitent l'emploi : [DBUnit](http://www.dbunit.org/) et [Jailer](http://jailer.sourceforge.net/).

### DBUnit
[DBUnit](http://www.dbunit.org/) est une extension de JUnit dont la tâche principale est de gérer des jeux de données qui seront retournés comme s'ils étaient le résultat d'une requête sur la base de données. Le premier avantage est de maintenir l'indépendance du code des tests vis-à-vis de la base de données réelle. Ensuite, DBUnit garantit que tous les tests unitaires retrouveront la base de données dans le même état et non dans l'état dans lequel le test précédent a pu la laisser.

Les jeux de données de DBUnit sont conservés sous forme XML. La première étape à son utilisation est donc de constituer les fichiers XML qui contienent les données (On pourra se référer au paragraphe suivant qui traite d'une méthode d'automatisation de cette étape). Voir ci-après un exemple de fichier XML de données :

{% highlight xml %}
<!DOCTYPE dataset SYSTEM "dataset.dtd">
<dataset>
  <table name="TEST_TABLE">
    <column>COL0</column>
    <column>COL1</column>
    <column>COL2</column>
    <row>
      <value>row 0 col 0</value>
      <value>row 0 col 1</value>
      <value>row 0 col 2</value>
    </row>
    <row>
      <null/>
      <value>row 1 col 1</value>
      <null/>
    </row>
  </table>
  <table name="SECOND_TABLE">
    <column>COLUMN0</column>
    <column>COLUMN1</column>
    <row>
      <value>row 0 col 0</value>
      <value>row 0 col 1</value>
    </row>
  </table>
  <table name='EMPTY_TABLE'>
    <column>COLUMN0</column>
    <column>COLUMN1</column>
  </table>
</dataset>
{% endhighlight %}

{% highlight xml %}
<!DOCTYPE dataset SYSTEM "dataset.dtd">
<dataset>
  <table name="TEST_TABLE">
    <column>COL0</column>
    <column>COL1</column>
    <column>COL2</column>
    <row>
      <value>row 0 col 0</value>
      <value>row 0 col 1</value>
      <value>row 0 col 2</value>
    </row>
    <row>
      <null/>
      <value>row 1 col 1</value>
      <null/>
    </row>
  </table>
  <table name="SECOND_TABLE">
    <column>COLUMN0</column>
    <column>COLUMN1</column>
    <row>
      <value>row 0 col 0</value>
      <value>row 0 col 1</value>
    </row>
  </table>
  <table name='EMPTY_TABLE'>
    <column>COLUMN0</column>
    <column>COLUMN1</column>
  </table>
</dataset>
{% endhighlight %}

Outil puissant et offrant une réelle valeur ajoutée, DBUnit devrait avoir sa place dans tous les environnements de tests. L'indépendance et la stabilité qu'il induit par rapport à la couche de persistance est un réel confort et un garant d'exécution correcte des tests. La mise en place est de plus simple. La constitution des fichiers XML peut cependant être fastidieuse et il est conseillé d'utiliser des outils automatiques de génération tels que Jailer.

### Jailer
Comme on l'a vu précédemment, il est utile de s'affranchir de la partie persistance quand on teste une application afin de s'assurer du déterminisme des réponses de requêtes (et aussi pour rendre les tests moins consommateurs en ressources). De ce point de vue, des outils comme DBUnit sont des *mocks* de base de données. Mais écrire les jeux de données de DBUnit peut être fastidieux et il vaut mieux souvent automatiser cette tâche. Dans ce sens, [Jailer](http://jailer.sourceforge.net/) permet d'extraire des jeux de tests DBUnit cohérents d'une base de données. La cohérence est assurée sur le plan des relations fonctionnelles des données tout en respectant les relations d'intégrité. Les sous-ensembles extraits présentent donc une « vue » pleinement utilisables par les tests et nettoyée des données superflues.

Jailer est une application lourde qu'un développeur lance et connecte à la base de données :

[![jailer\_settings]({{ site.url }}/images/2010/07/jailer_settings.png)]({{ site.url }}/images/2010/07/jailer_settings.png)

Après quelques paramétrages optionnels de plus, Jailer permet la sélection de données choisies comme le montre la capture suivante d'une base exemple d'employés :

[![jailer\_export]({{ site.url }}/images/2010/07/jailer_export.png)]({{ site.url }}/images/2010/07/jailer_export.png)

Dans la capture précédente issue du site officiel, on a demandé un export SQL mais on peut voir que l'outil permet de créer un fichier XML (au format DBUnit).

Jailer est simple d'emploi et il permet de créer voir d'enrichir des jeux de données au fil des développements très rapidement. Le gain de temps qu'il assure au développeur est conséquent et il lui évite une tâche laborieuse et rébarbative. Allié à DBUnit, Jailer est l'outil parfait pour envisager des tests unitaires de façon sereine. Pas de problème de cohérence ou de déterminisme des données.

Le binôme DBUnit-Jailer apporte la flexibilité et la rapidité de mise en place aux tests unitaires. Pas d'hésitation donc sur ces deux outils : les tests, même s'ils devraient toujours être un passage obligatoire dans un projet informatique ne sont pas une finalité. Tout processus visant à les accélérer ou les faciliter est donc bon à prendre.

*Article publié sur [LinuxFr](http://linuxfr.org/~galaux/) dans le cadre de mon activité professionnelle à [Linagora](http://linagora.com/).*

