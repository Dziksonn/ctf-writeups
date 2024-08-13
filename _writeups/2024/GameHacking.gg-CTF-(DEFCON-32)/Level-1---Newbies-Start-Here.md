---
layout: writeup
category: GameHacking.gg-CTF-(DEFCON-32)
chall_description: T
points: 100
solves:
tags: unity cheat-engine
date: 2024-08-12
comments: true
---

First thing We see in first level is pistol with health bar and box around it:
![Pistol](https://imgur.com/qHQcKGG.png)

Trying to pick it up, laser nearby dealt 10 damage to us, but also we dealt some damage to pistol.

Two ideas, disable laser, or be able to survive laser attacks. Being able to survive it seems easier, so let's try to find way to change health by [Cheat Engine](https://www.cheatengine.org/).

Selecting process GameHackingGG in cheat engine, value type: all, scan type: exact value, value: 90, first scan. 1,690 results found, we need to narrow it down. Let's take some damage, to change this value. Bumped another time in pistol, now Im at 80 HP, so I type 80 in value, hit Next Scan. 3 Results left, and 2 of them already changed.

![HP Values](https://imgur.com/T2lQ8h1.png)

Second one is value that we are looking for, double click on it to move it to bottom, click Active box to freeze it at 80. Now we can pick up pistol without taking damage.

![Flag](https://imgur.com/hjQxQw9.png)

And we have our Flag, pick it up, click `I` to open inventory, and there it is.
`GHCTF{this_is_the_first_flag_woot}`


[Jump to next one](./Level-2.html)

