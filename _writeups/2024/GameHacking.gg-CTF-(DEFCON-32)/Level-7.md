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

In this level, things are different, I got hit with large popout window "Cheater Detected!"
![Cheater Detected](https://imgur.com/Y4s9oRV.png)
And after few seconds game closes.

Looking at dnSpy there is `anticheat` class, let's take look into it. There is array `knownCheatTools` that contain names of known tools used for cheating. I will just change it to one string with random letters in it (don't want to leave it empty, because something might break). Save module, restart game.

Now I can play game without any interruptions. There is whack-a-mole type game, I don't even try to finish it legit way. I just look again in dnSpy and look after anythin related.

And there is `wackamole` class with alot of methods, but there is one that looks more interesting and it is `CheckWin` method that looks like this:
```csharp
private bool CheckWin()
{
	Debug.Log(string.Format("{0}:{1}", this.spawnCount, this.killCount));
	if (this.spawnCount == this.killCount && this.spawnCount != 0)
	{
		this.completionFeedback.PlayFeedbacks();
		global::UnityEngine.Object.Instantiate<GameObject>(this.moleFlag, base.transform.position, Quaternion.identity);
		return true;
	}
	this.shootingFeedback.PlayFeedbacks();
	base.StartCoroutine(this.WaitDelay(0.5f));
	this.playerHealth.Kill();
	return false;
}
```
I'll change `if (this.spawnCount == this.killCount && this.spawnCount != 0)` to `if (this.spawnCount > 0)`. Save module, restart game.

Starting wack-a-mole game, and after few seconds flag spawned. Picking it up reveals `GHCTF{nice_shooting_tex}`.

[Jump to next one](./Level-8.html)