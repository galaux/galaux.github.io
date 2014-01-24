---
layout: post
title: Jail Skype!
description: "How to prevent a bad piece of software from spying on you"
category: articles
tags: [security]
---

[Skype](http://www.skype.com/intl/fr/home) is the most used voip software used accross the world. Unfortunately when you dig a bit about it you may find yourself surprised with its developers' way of understanding the word "privacy". Reports have been made about [Skype binaries reading sensitive Linux password files](http://forum.skype.com/index.php?showtopic=95261) and [user personal data](http://yro.slashdot.org/story/07/08/26/1312256/Skype-Linux-Reads-Password-and-Firefox-Profile). As Skype is the perfect example of closed source application one cannot even know if these statements are true. Moreover, it seems Skype makes heavy use of [obfuscation methods](http://en.wikipedia.org/wiki/Skype_protocol#Obfuscation_Layer). It just so happens an [Archer](http://www.archlinux.org) explained this [simple way of jailing Skype into a reserved Linux account](https://wiki.archlinux.org/index.php/Skype#Use_Skype_with_special_user).

The idea is very simple: create a `skype` group and a `skype` user with its own home directory. Then just setup your regular user to launch the `skype` binary as this user using `sudo`. If you grant this `skype` user restricted rw rights, this will elegantly jail this impolite Skype software.

