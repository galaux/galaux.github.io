---
layout: post
title: Jetty 7 à la rescousse
description: ""
category: articles
tags: [java,server]
---

[![JETTY](http://08000linux.com/blogs/files/2009/12/JETTY.gif)Jetty](http://jetty.codehaus.org/jetty/) est le serveur HTTP et conteneur de servlet qu'il fallait suivre ces dernières années. Il a été massivement adopté dans les milieux professionnels pour sa légèreté, sa polyvalence et sa robustesse. Ses développeurs proposent depuis quelques temps sa [version 7](http://www.eclipse.org/jetty/about.php) considérée comme stable. Celle-ci devait implémenter la spécification 3.0 de Servlet mais le retard de la spécification a poussé son créateur à proposer cette version "intermédiaire".

La principale nouveautés concerne le réarrangement de *packages* en *bundle OSGi* par rapport à la version 6. Ces changements ont été documentés par l'équipe de développement et il suffira de se reporter aux différents [articles du site officiel](http://wiki.eclipse.org/Jetty/Starting#Upgrades) pour s'y retrouver. Jetty 7 implémente donc la spécification 2.5 des Servlets mais propose aussi quelques avant-goûts de la 3.0. On peut par exemple citer les transactions client/serveur asynchrones via une implémentation de [WebSocket](http://www.w3.org/TR/2009/WD-websockets-20091222/), une API et un protocole issus du travail du groupe [Whatwg](http://www.whatwg.org/) sur [HTML5](http://dev.w3.org/html5/spec/Overview.html).

Les nouveautés ne sont donc peut-être pas aussi importantes que celles prévues pour la version 8, qui est déjà dans les cartons. Mais ce pas en avant est notable quand on parle d'un composant aussi utilisé et de fait attendu que Jetty. Notons enfin que cette version 7 est la première depuis que la fondation Eclipse a pris Jetty sous son aile, ce qui le rend maintenant [disponible sous les licences apache 2.0 ou eclipse 1.0](http://www.eclipse.org/jetty/licenses.php).

*Article publié sur [LinuxFr](http://linuxfr.org/~galaux/) dans le cadre de mon activité professionnelle à [Linagora](http://linagora.com/).*

