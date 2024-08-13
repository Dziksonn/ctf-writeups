---
layout: writeup
category: GameHacking.gg-CTF-(DEFCON-32)
chall_description: T
points: 300
solves:
tags: unity dnSpy
date: 2024-08-12
comments: true
---

In this level, there is floor that fall down when trying to get flag. Probably normal approach would be to make some kind of fly hack, but I'm not too familiar with Cheat Engine, so I choose to do it with [dnSpy](https://github.com/dnSpy/dnSpy/releases).

Launching dnSpy, select all files from `./GameHackingGG_Data/Managed` and throw them into dnSpy. First thing I want to check will be `Assembly-CSharp.dll` and looking for anything related to falling floor. And there it is `PitFall` class with `OnTriggerEnter` method that looks like this:


```csharp
private void OnTriggerEnter(Collider other)
{
	if (other.CompareTag("Player"))
	{
		this.floor.SetActive(false);
		this.floor2.SetActive(false);
		GameObject[] array = this.pitObjects;
		for (int i = 0; i < array.Length; i++)
		{
			array[i].SetActive(true);
		}
		global::UnityEngine.Object.Destroy(this.flag);
	}
}
```
Changing `this.floor.SetActive(false);` to `this.floor.SetActive(true);` and deleting `global::UnityEngine.Object.Destroy(this.flag);`. Save module, restart game.

Now character doesnt fall when entering floor, flag is not destroyed, but can't be picked up. So I need to search for some additional check, or something like that. Right on top of PitFall class, there is `PitComplete` class. There is `OnTriggerEnter` method that spawns another flag.

```csharp
private void OnTriggerEnter(Collider other)
{
	if (other.CompareTag("Player") && !this.completed)
	{
		this.completionFeedbacks.PlayFeedbacks();
		global::UnityEngine.Object.Instantiate<GameObject>(this.pitflag, base.transform.position, Quaternion.identity);
		this.completed = true;
	}
}
```

I'll try to run around and maybe find area that trigger this function. Running close to exit spawns another flag. This time I can pick it up and it says `GHCTF{You_should_play_basketball}`.

[Jump to next one](./Level-5.html)



