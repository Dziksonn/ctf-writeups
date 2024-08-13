---
layout: writeup
category: GameHacking.gg-CTF-(DEFCON-32)
chall_description: T
points: 500
solves:
tags: unity dnSpy
date: 2024-08-12
comments: true
---

Where is level 10? There is no level 10!

In level selector there is no level 10, in level 9 there was no way to go to next level. So we probably need to look in dnSpy for it.

Looking around at `NextLevel` and `MainMenu` I see that in MainMenu there is method `LoadLevel` that looks like this:
```csharp
public void LoadLevel(int level)
{
	SceneManager.LoadScene(level);
}
```

So I think that might be useful to mess with. First idea, try to make it that if I select level 1 it will load me to level 10. Should be easy, just simple if and else statement like this:
```csharp
	public void LoadLevel(int level)
	{
		if (level == 1){
			SceneManager.LoadScene(10);
		} else {
		SceneManager.LoadScene(level);
		}
	}
```

And now when Level 1 selected it should load me to level 10. And it does!

I see chest, with E key prompt, when E pressed it does only play _error/spring_ sound and nothing else.

Looking at DnSpy again I see `chest` class that wasn't used in anything before. There is `OpenChest` method that looks like this:
```csharp
public void OpenChest()
{
	if (this.playerInventory.InventoryContains("Key").Count < 1)
	{
		this.failFeedbacks.PlayFeedbacks();
		return;
	}
	this.chestObj.SetActive(false);
	this.completionFeedbacks.PlayFeedbacks();
	global::UnityEngine.Object.Instantiate<GameObject>(this.hiddenFlag, base.transform.position, Quaternion.identity);
}
```
Just deleting `return;` from it should solve problem. Save module, restart game. Pressing E on chest reveals `GHCTF{well_played_hacker_gg}`.

[Go back to main page](./index.html)
