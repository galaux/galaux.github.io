---
layout: post
title: Spring : présentation
description: ""
category: articles
tags: [java,development]
---

![logo](http://08000linux.com/blogs/files/2010/01/logo1.png)La version 3 de [Spring](http://www.springsource.org/) a récemment été annoncée ! Le framework Java EE de la communauté Spring présente des ajouts intéressants par rapports à ses versions précédentes.

La nouveauté la plus marquante est le support du JDK 1.5. Spring 3 a été ré-écrit avec cette version du JDK et assure ainsi la compatibilité. Le JDK 1.5 devient par là même un pré-requis à l'utilisation de Spring 3. Les développeurs de Spring ont aussi profité de cette montée de version pour faire du ménage : certaines classes trop anciennes sont maintenant marquées comme dépréciées et d'autres ont été supprimées lors de ce processus normal d'évolution.

Les auteurs de Spring ont aussi largement utilisé le principe [REST](http://fr.wikipedia.org/wiki/Representational_State_Transfer) (REpresentational State Transfer). L'idée derrière ce n-ième acronyme est d'associer une URL à chaque ressource et de la rendre ainsi facilement accessible dans l'application web.

Afin de simplifier le paramétrage des applications développées, Spring s'est doté d'un Expression Language (EL). Ce langage spécifique au framework apporte une souplesse et une certaine puissance comme le montre l'exemple suivant (extrait de cet [article en ligne](http://www.h-online.com/open/features/Expression-Language-746873.html)). On y voit comment utiliser la propriété système "Pays" de l'environnement d'exécution via la syntaxe \#{systemProperties.country} : `@Component public class CountryAwareLogic {     ...     @Autowired     public void setCountry(         @Value("#{systemProperties.country}") String country) {         this.country=country;     } }`

Spring 3 implémente maintenant les [HTML ETag](http://en.wikipedia.org/wiki/HTTP_ETag) (entity tag). Cette fonctionalité du protocole HTTP permet d'envoyer au clients HTTP des réponses du type "304 (not modified)" pour renseigner par exemple sur l'état d'une page re-demandée et ainsi éviter un retransmission inutile.

Parmi les autres nouveautés Spring, on peut aussi citer le support de Portlet 2.0 et des "conversations web" dont le but est de permettre l'utilisation de variables avec une portée (*scope*) de plusieurs requêtes. Le [site de la communauté Spring](http://static.springsource.org/spring-security/site/changelog.html) propose la liste exhaustive des changements à cette adresse.

*Article publié sur [LinuxFr](http://linuxfr.org/~galaux/) dans le cadre de mon activité professionnelle à [Linagora](http://linagora.com/).*

