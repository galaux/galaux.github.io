---
layout: post
title: Quoi de beau dans la version 7 de Tomcat?
description: "Les nouveautés de la prochaine version de Tomcat décortiquées"
category: articles
tags: [java,server]
---

La fondation Apache travaille toujours sur son serveur d'applications Tomcat et nous pouvons voir transpirer sur le [site de la fondation](http://wiki.apache.org/tomcat/TomcatVersions) certaines informations sur la prochaine version. Voyons donc quels sont les points qui seront au menu de Tomcat 7 décris pour l'instant comme *preview*.

### Quoi de neuf?
Tomcat 7 *preview* apportera certaines nouveautés comme par exemple la communication en cluster via UDP, le remplacement du mécanismes des Valve par des filtres et l'amélioration du support JMX. Passons sur les [nettoyages de code et corrections de bugs](http://svn.eu.apache.org/repos/asf/tomcat/trunk/webapps/docs/changelog.xmlhttp://) habituels dans une version majeure pour arriver au coeur du sujet : les version des composants supportés. La nouvelle version Tomcat devra s'accompagner d'un JDK 6 ou supérieur. Elle implémentera la spécification 2.1 des JSP mais surtout comme nous allons le voir plus en détails, la spécification 3.0 des Servlet.

### Servlet 3.0
Parmi les nombreuses améliorations apportées par l'implémentation de la spécification Servlet 3.0, on trouvera le traitement asynchrone des tâches. Ceci a pour but de séparer les traitements induits par le container de ceux résultants d'objets ServletRequest/ServletReponse. Attention toutefois le but d'un tel système n'est pas d'éviter l'interbloquage des entrées/sorties de servlet. De façon pratique, la possibilité de traiter des tâches de façon asynchrone apportera la possibilité de rediriger une requête vers une URL, assigner une tâche à un processus pour exécution ou encore informer un container de la complétion d'une tâche.

{% highlight java %}
@WebFilter(asyncSupported=true)
public class MyFilter {
}

@WebServlet(asyncSupported=true)
public class MyServlet {
}

service(Request req, Response res) {
  AsyncContext actx = req.startAsync();
  Runnable runnable = new Runnable() {
    public void run() {
      Message m = jmsTemplate.receive();
      res.write(m);
      req.complete();
    }
  };

  executor.submit(runnable);
}
{% endhighlight %}

La configuration dynamique est une nouveauté de cette version de Tomcat. Comme son nom l'indique, cette nouvelle capacité de Tomcat permettra d'ajouter à *run-time* des servlets ou des filtres. Cette opération ne pourra être faite qu'à l'initialisation du contexte de la servlet (ServletContextListener.contextInitialized()) comme montré dans l'exemple suivant.

{% highlight java %}
interface Servlet/Filter-Registration{
  setDescription(String);
  setInitParameter(String name,Object value);
  setInitParameters(Map p);
  setAsyncSupported(boolean supported);
  addMappingForUrlPatterns(...);
}
{% endhighlight %}

Qui n'a pas rêvé de fractionner la configuration générale de votre serveur Tomcat en plusieurs fichiers? On sera servi avec cette version 7 *preview* car elle ajoute une fonctionnalité de Web Fragments. Il sera possible, par exemple, d'inclure dans les JARs appropriés une configuration spécifique.

Certaines annotations ont été rajoutées afin de simplifier la déclaration d'instances ou de classes. Leurs noms parle d'eux-même :

{% highlight java %}
@WebServlet (must extend HttpServlet)
@WebFilter (must implement Filter)
@WebInitParam (both servlets/filters)
@WebListener
- ServletContextListener
- HttpSessionListener
- ServletRequestListener
{% endhighlight %}

Une nouvelle option *run-time* fait son apparition dans Tomcat 7 *preview* : l'authentification "programmatique" d'utilisateur via les méthodes suivantes :

{% highlight java %}
login(HttpServletResponse resp);
login(String username, String password);
{% endhighlight %}

Dernière nouveauté qu'on mentionnera ici, la configuration des cookies de sessions apportera la possibilité de modifier certain paramètres de cookies de session via les méthodes de l'interface SessionCookieConfig.

{% highlight java %}
interface javax.servlet.SessionCookieConfig {
  setName(String name);
  setSecure(boolean isSecure);
  setHttpOnly(boolean isHttpOnly);
  setPath(String path);
  setDomain(String domain);
  setComment(String comment);
}
{% endhighlight %}

On peut se référer à [ce document](http://svn.apache.org/repos/asf/tomcat/trunk/TOMCAT-7-RELEASE-PLAN.txt) de la fondation Apache qui décrit l'état des développements vers l'implémentation complète de la spécification Servlet 3.0. Gageons que la prochaine version de la [conférence Apache ApacheCon](http://www.us.apachecon.com/c/acus2009/schedule#tomcat) lèvera d'autres pans du voile sur les fonctionnalités de ce futur Tomcat 7.

*Article publié sur [LinuxFr](http://linuxfr.org/~galaux/) dans le cadre de mon activité professionnelle à [Linagora](http://linagora.com/).*

