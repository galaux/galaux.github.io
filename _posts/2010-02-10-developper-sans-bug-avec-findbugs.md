---
layout: post
title: Développer sans bug avec FindBugs?
description: ""
category: articles
tags: [java,tests,development]
---

![buggy-sm](http://08000linux.com/blogs/files/2010/01/buggy-sm.png)![informal](http://08000linux.com/blogs/files/2010/01/informal.png)Il existe nombre d'outils d'audit de code dans l'univers du développement Java qui sont autant d'armes contre les faux-pas de développement. [FindBugs](http://findbugs.sourceforge.net/) (qui porte bien son nom...) propose de parcourir les applications java à la recherche de motifs à problèmes.

L'outil développé par l'Université du Maryland appartient à la famille des analyseurs statiques comme [PMD](http://pmd.sourceforge.net/) ou [CheckStyle](http://checkstyle.sourceforge.net/). Il ne cherche pas à observer l'application en action mais plutôt à se plonger dans son code. Cependant FindBugs se différencie au sein de cette famille par le fait que son analyse est effectuée sur le *bytecode* plutôt que sur le code source.

Findbugs détecte une liste de *bugs* connus, sans cesse remise à jour, qui va de la simple [comparaison de chaînes de caractères avec l'opérateur "="](http://findbugs.sourceforge.net/bugDescriptions.html#ES_COMPARING_STRINGS_WITH_EQ) jusqu'à des [*bugs* en environnement *multi-threads* plus complexes](http://findbugs.sourceforge.net/bugDescriptions.html#ML_SYNC_ON_FIELD_TO_GUARD_CHANGING_THAT_FIELD). Si la [liste complète des problèmes recherchés](http://findbugs.sourceforge.net/bugDescriptions.html) ne donne pas satisfaction, FindBugs offre la possibilité de créer des *plugins* pour l'augmenter. Le [*plugin* fb-contrib](http://fb-contrib.sourceforge.net/bugdescriptions.html) permet en particulier l'identification de nombreux cas qui n'ont pas encore trouvé le chemin de la liste officielle.

[![FindBugs\_Eclipse](http://08000linux.com/blogs/files/2010/01/FindBugs_Eclipse1.png)](http://08000linux.com/blogs/files/2010/01/FindBugs_Eclipse1.png)

L'Université du Maryland propose son outil sous diverses formes qui vont de l'application *standalone* jusqu'au [JNLP](http://fr.wikipedia.org/wiki/JNLP) en-ligne en passant par le *plugin* Eclipse (via l'URL d'installation http://findbugs.cs.umd.edu/eclipse). Les tâches Ant, Maven ou un *plugin* Sonar permettent quant à eux de l'inclure dans un processus d'intégration continue.

Pour conclure, on peut noter que Google l'utilise lors de ses "FixIt Days". Ce sont des jours dans l'année dédiée à la résolution des TODO, des notes et des vices cachés des programmes que les développeurs de Google écrivent. Cette pratique n'est pas encore très suivie dans toutes les entreprises, mais semble pertinente quand l'on constate la qualité quelques logiciels...

*Article publié sur [LinuxFr](http://linuxfr.org/~galaux/) dans le cadre de mon activité professionnelle à [Linagora](http://linagora.com/).*

