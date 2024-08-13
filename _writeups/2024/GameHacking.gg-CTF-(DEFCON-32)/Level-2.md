---
layout: writeup
category: GameHacking.gg-CTF-(DEFCON-32)
chall_description: T
points: 200
solves:
tags: unity cheat-engine
date: 2024-08-12
comments: true
---

We have pistol with 10 ammo, and there is unicorn with health bar:
![Unicorn](https://imgur.com/g2zjVZ0.png)

10 bullets isn't enough, so we will need more ammo. Let's try to find it with Cheat Engine.

I restart level, and scan for 10 ammo, type 10 in value, hit First Scan. 49531 results found.

Shoot, change value, next scan, repeat. 3 results left. Shooting again, and they all change to amount of ammo that I have.

Let's move them all to bottom, and click active box next to them. And now we have unlimited ammo.

Unicorn killed, and we have flag `GHCTF{oops_sorry_mr_unicorn}`.


[Jump to next one](./Level-3.html)