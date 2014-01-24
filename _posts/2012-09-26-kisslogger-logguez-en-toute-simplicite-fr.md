---
layout: post
title: KissLogger, logguez en toute simplicité
description: "Une classe KISS pour logger dans une application CLI Java"
category: articles
tags: [java,development]
---

[Le projet *test* Dios Mio](http://www.alaux.net/index.php?article34/dios-mio-ecorche-d-une-webapp-fr) que j'utilisais comme application de test comporte entre autres une interface en ligne de commande (CLI) pour laquelle j'ai évidemment eu besoin d'écrire des logs. Mes besoins étaient extrêmement simples : logguer dans un fichier aux différents niveaux habituels (`ERROR`, `WARN`, ...). Utiliser une librairie externe pour si peu semblait inutile, j'ai donc écrit une simple classe Java qui remplit cette fonction : `KissLogger`.

Un autre besoin est apparu par la suite. Je souhaitais disposer immédiatement après le lancement de l'application CLI d'un logger et ceci avant même d'avoir traité les options passées par l'utilisateur en paramètre de ligne de commande ou même d'avoir lu le fichier de configuration. Or ce n'est qu'une fois ces deux actions effectuées que la destination final des lignes de log est connue. J'ai donc modifié cette classe pour qu'elle écrive initialement dans un buffer en mémoire le temps de traiter les options passées en ligne de commande et de lire le fichier de configuration (ou n'importe quelle autre action d'ailleurs). `KissLogger` propose la méthode `switchDestination(PrintStream destination)` qui permet par la suite de basculer la destinaton des lignes de log à `run time`. Celle-ci vide alors le buffer dans la destination spécifiée et configure le logguer pour utiliser ce fichier à l'avenir.

Le déroulement suivant est donc possible :

{% highlight java %}
public static void main(String[] args) {
  KissLogger logger = new KissLogger(KissLogger.Level.DEBUG);
  logger.info("Application starting...");

  // Traitement des options de la ligne de commande où l'utilisateur
  // peut spécifier "--log-dest /var/log/test.log" afin de logguer
  // dans un fichier précis
  logger.info("User specified an alternate log dest");
  ...

  // voir même il peut avoir spécifié "--config-file conf-perso.conf"
  // un fichier de configuration alternatif dans lequel il a précisé
  // le fichier dans lequel les logs doivent être écrits
  logger.info("User specified an alternate config file.");
  logger.warn("Log destination from configuration file will be overriden");
  ...

  // Lecture du fichier de configuration par défaut
  // ou de celui précisé par l'utilisateur
  logger.info("Reading configuration file " + configFile.getAbsolutePath());
  ...

  // OK toutes les options ont été traitées,
  // l'utilisateur veut logguer dans logFinalDest :
  logger.switchDestination(logFinalDest);

  // Suite de l'application
  ...

  // Fin de l'application
  logger.debug("Application done.");
}
{% endhighlight %}

Ce qui donnera les lignes de log suivantes dans le fichier `logFinalDest` :

    2012/09/24 18:27:32 INFO - Application starting...
    2012/09/24 18:27:32 INFO - User specified an alternate log dest
    2012/09/24 18:27:32 INFO - User specified an alternate config file.
    2012/09/24 18:27:32 WARN - Log destination from configuration file will be overriden
    2012/09/24 18:27:32 INFO - Reading configuration file conf-perso.conf
    ...
    2012/09/24 18:27:32 DEBUG - Application done.

La classe `KissLogger` permet aussi de changer le format de la date affichée en tête de ligne dans les logs, de changer le niveau de log à `run time` et dispose d'une méthode qui loggue une `stacktrace` complète.

Cette classe n'est pas géniale en soi, elle est même finalement très bête mais elle remplit complètement son office et le tout en quelques dizaines de lignes de code. C'est la définition même du *[Keep It Simple Stupid](http://fr.wikipedia.org/wiki/Principe_KISS#En_informatique)*!

"*There are two ways of constructing a software design: One way is to make it so simple that there are obviously no deficiencies, and the other way is to make it so complicated that there are no obvious deficiencies. The first method is far more difficult.*"
 -- [Sir Tony Hoare](http://en.wikipedia.org/wiki/Tony_Hoare)

