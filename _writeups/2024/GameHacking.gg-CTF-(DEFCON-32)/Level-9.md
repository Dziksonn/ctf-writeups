---
layout: writeup
category: GameHacking.gg-CTF-(DEFCON-32)
chall_description: T
points: 400
solves:
tags: unity dnSpy
date: 2024-08-12
comments: true
---

This time instead of normal platform, we are spawned on race track. We have 10 seconds to complete it, and that isn't enough so turret shoots and kills us after 10 seconds.

Looking at classes, next to `LazerTurret` that we changed in Level 5 is `LazerTurretRace` class. I'll look into it.
And the first method that catches my eye is `RemoteKill` method that looks like this:
```csharp
public void RemoteKill(Health playerHealth)
{
	global::UnityEngine.Object.Instantiate<ParticleSystem>(this.lazer, playerHealth.transform.position, Quaternion.identity);
	this.lazerTurretMuzzle.Play();
	playerHealth.Kill();
}
```
I'll delete `playerHealth.Kill();` from this method, and try race again. I was able to complete race and flag spawned. Picking it up reveals `GHCTF{zoom_zoom_money_maker}`.

[Jump to next one](./Level-10-question-mark.html)