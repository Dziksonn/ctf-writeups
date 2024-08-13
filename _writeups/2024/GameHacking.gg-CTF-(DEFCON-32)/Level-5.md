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

We see flag that is covered by red box, and if player hit this red box the laser shoots and kill player instantly.
![Red Box](https://imgur.com/b8Idn23.png)

Looking for some kind of box or laser logic in dnSpy. I found `LazerTurret` class with `FireLazer` method that looks like this:
```csharp
	private void FireLazer()
	{
		this.playerHealth = GameObject.FindWithTag("Player").GetComponent<Health>();
		global::UnityEngine.Object.Instantiate<ParticleSystem>(this.lazer, this.playerHealth.transform.position, Quaternion.identity);
		this.lazerTurretMuzzle.Play();
		if (this.doesKill)
		{
			this.playerHealth.Kill();
		}
	}
```
So I delete everything from inside `FireLazer` method and it looks now like this:
```csharp
	private void FireLazer()
	{
	}
```

Save module, restart game. Now I can jump into red box and nothing happens. I can pick up flag and it says `GHCTF{OK_now_you_have_godmode}`.

[Jump to next one](./Level-6.html)