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

Unicorn tells us, that to pass we need Fish, but we are only given apple and there is statue that we should give fish to pass level.
![Fish](https://imgur.com/dmWku5e.png)

Looking in dnSpy I see `hasFish` class with `OnTriggerEnter` method that looks like this:
```csharp
private void OnTriggerEnter(Collider other)
{
	if (other.CompareTag("Player"))
	{
		if (this.inventory == null)
		{
			Debug.LogError("Could not get player inventory");
			return;
		}
		this.fishItems = this.inventory.InventoryContains("Fish");
		if (this.fishItems.Count > 0)
		{
			this.completedFeedback.PlayFeedbacks();
			global::UnityEngine.Object.Instantiate<GameObject>(this.fishFlag, base.transform.position, Quaternion.identity);
			GameObject[] array = this.completionObjects;
			for (int i = 0; i < array.Length; i++)
			{
				array[i].SetActive(false);
			}
			array = this.antiCheatWarnings;
			for (int i = 0; i < array.Length; i++)
			{
				array[i].SetActive(true);
			}
			return;
		}
		Debug.Log("No fish found :(");
	}
}
```
`if (this.fishItems.Count > 0)` looks like thing that I need to change. I'll change it to `if (this.fishItems.Count >= 0)` so it will require 0 fish or more, instead of more than 0. Save module, restart game.

Stand on yellow platform, and there it is. Statue disappeared and flag spawned. Picking it up reveals `GHCTF{Kitty_appreciates_the_fish_magic}`.

[Jump to next one](./Level-7.html)