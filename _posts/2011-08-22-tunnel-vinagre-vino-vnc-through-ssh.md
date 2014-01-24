---
layout: post
title: Tunnel vinagre-vino (VNC) through SSH
description: "Encrypt data of your VNC desktop sharing sessions"
category: articles
tags: [desktop,security]
---

Remote desktop connections with [VNC](http://en.wikipedia.org/wiki/Virtual_Network_Computing) is very useful when you need to help regular users stuck on their desktop environment. [Gnome](http://en.wikipedia.org/wiki/GNOME) provides vino-server and its client side vinagre as VNC implementations. But vino is not fully secured. The password check between client and server looks OK but once the connection is made, the stream between the two is not encrypted. Using it through internet may be risky so why not tunnel it all through [SSH](http://en.wikipedia.org/wiki/Secure_Shell)?

### Pros and cons

- Pros:
  - Harden vino authentication
  - Encrypt stream between client and server

- Cons:
  - A little bit geeky I agree but hey you are still reading!

### How to procede

First of all we need to install vino an openssh on the server and vinagre and openssh on the client (Debian/Ubuntu users must be careful there: we need OpenSSH server part on the server and on the client side we need both the client and the server part). [Archers](http://www.archlinux.org) can run:

On the server side:

{% highlight bash %}
pacman -Sy vino openssh
{% endhighlight %}

On the client side:

{% highlight bash %}
pacman -Sy vinagre openssh
{% endhighlight %}

We are then going to setup the server side: vino. This can be done with `vino-preferences`: allow connections, and specify a password (this is optional as we will tunnel everything).

On the 'client' side, we need to forward some SSH to our host (here specified as the variable `${REMOTE_HOST}`):

{% highlight bash %}
ssh -C -NL 9999:localhost:5900 ${REMOTE_HOST} &
{% endhighlight %}

- `-L`: parameter for port forwarding from port `9999` on `localhost` and forward it to port `5900` to `REMOTE_HOST`. Port `5900` is the vino default port. Port `9999` can be changed to any port you are allowed to use
- `-N` do not execute a remote command (ie "you do not get a prompt on the remote machine")
- `-C`: compress data (optional)

On the client side we can now launch any VNC client and connect to `localhost:9999`. With vinagre this would be:

{% highlight bash %}
vinagre ::9999
{% endhighlight %}

You now should be prompted for your password if you set one up on the previous step. The client side may also be prompted for a connection authorization depending on what you configured. Click OK and you should see your remote desktop on a window.

Last but not least, let's prevent connections to vino-server on the remote side from other places than localhost. It seems this option used to be available in vino-preferences but it disappeared in the latest versions. We are thus goinf to use dconf-editor to setup this:

{% highlight bash %}
dconf-editor
{% endhighlight %}

Navigate the tree to the leaf `desktop/gnome/remote-access` and set value `network-interface` to `lo`. This network interface is of course the localhost you can see when issuing `ifconfig lo`.

There. You might need to re-login or at least restart `/usr/lib/vino/vino-server` for these last options to take effects. Once done you should get your fully secured VNC connection tunneled to SSH.

Note: Do not forget to kill the port forwarding after you are done using vinagre.

For automation, here is a script summarizing the client side:

{% highlight bash %}
#!/bin/bash
LOCAL_PORT=9999
REMOTE_HOST=192.168.1.21
REMOTE_PORT=5900

echo "Opening SSH tunnel"
# Do not use '-f' because '&' already forces background
# '-f' disables PID retrieval
ssh -C -NL ${LOCAL_PORT}:localhost:${REMOTE_PORT} ${REMOTE_HOST} &
SSH_PID=$!
echo "Connecting to ${REMOTE_HOST}"
vinagre ::${LOCAL_PORT}
kill ${SSH_PID}
{% endhighlight %}

