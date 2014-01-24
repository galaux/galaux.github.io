---
layout: post
title: systemd unit files and "environment variables"
description: "Useful tip about systemd '.service' syntax"
category: articles
tags: [java,archlinux]
---

I recently got stuck with a simple systemd detail about environment variables processing. They are actually handled differently from any regular bash variable we are used to deal with.

I was trying to write the Tomcat 7 service unit for [Arch Linux](http://www.archlinux.org/packages/extra/any/tomcat7/). Everything worked fine except for the `CATALINA_OPTS` variable not beeing correctly interpreted when made of multiple parameters. The usual JVM heap options `-Xms512m -Xmx1024m` for instance were not correctly handled: only the first parameter was passed on to Tomcat.

This was one of my first attempt: Unit file:

{% highlight bash %}
...
[Service]
...
EnvironmentFile=/etc/conf.d/tomcat7
ExecStart=/usr/share/tomcat7/bin/startup.sh ${CATALINA_OPTS}
ExecStop=/usr/share/tomcat7/bin/shutdown.sh
...
{% endhighlight %}

`/etc/conf.d/tomcat7` conf file:

{% highlight bash %}
...
CATALINA_OPTS=-Xms512m -Xmx1024m
...
{% endhighlight %}

This does not work because environment variables in systemd are handled differently if used with `${MYVAR}` or with `$MYVAR`.

- `${MYVAR}` will provide the target process with a single string argument made of the complete string you specified in `MYVAR`.
- `$MYVAR` in the other hand will split `MYVAR` around spaces it contains and feed them to the process.

I have tried many things like surrounding the `CATALINA_OPTS` with simple quotes, double-quotes until [this blog post](http://patrakov.blogspot.fr/2011/01/writing-systemd-service-files.html%20gave%20me%20the%20solution).

So the correct unit file

{% highlight bash %}
...
[Service]
...
EnvironmentFile=/etc/conf.d/tomcat7
ExecStart=/usr/share/tomcat7/bin/startup.sh $CATALINA_OPTS
ExecStop=/usr/share/tomcat7/bin/shutdown.sh
...
{% endhighlight %}

Have a look at [the final unit file](https://projects.archlinux.org/svntogit/packages.git/tree/trunk/systemd.tomcat7.service?h=packages/tomcat7) and [the corresponding configuration file](https://projects.archlinux.org/svntogit/packages.git/tree/trunk/tomcat7.conf.d?h=packages/tomcat7) for a complete example.

