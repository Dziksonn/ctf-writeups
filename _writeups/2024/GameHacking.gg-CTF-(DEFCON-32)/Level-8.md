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

There is a chest, with E key prompt, when E pressed it spawns random items. I'll probably need flag from it, and it doesn't drop after few clicks so it probably have really low chances. I'll look into dnSpy for some kind of logic that is responsible for spawning items.

I see `lottery_check`, but nothing interesting inside. There is also `chest` class, but nothing interesting inside either. I'll search for **loot** or **loot_table** or similars.

After looking few minutes around functions and classes, I found `MMLootTable` class with `ComputeWeights` method in `MoreMountains.Tools` that looks like this:
```csharp
		public virtual void ComputeWeights()
		{
			if (this.ObjectsToLoot == null)
			{
				return;
			}
			if (this.ObjectsToLoot.Count == 0)
			{
				return;
			}
			this._maximumWeightSoFar = 0f;
			foreach (T t in this.ObjectsToLoot)
			{
				if (t.Weight >= 0f)
				{
					t.RangeFrom = this._maximumWeightSoFar;
					this._maximumWeightSoFar += t.Weight;
					t.RangeTo = this._maximumWeightSoFar;
				}
				else
				{
					t.Weight = 0f;
				}
			}
			this.WeightsTotal = this._maximumWeightSoFar;
			foreach (T t2 in this.ObjectsToLoot)
			{
				t2.ChancePercentage = t2.Weight / this.WeightsTotal * 100f;
			}
			this._weightsComputed = true;
		}
```
I'll be honest, I used ChatGPT to understand better what could I change to make all weights the same. (Probably I was tired already)

ChatGPT wrote this:
```csharp
public virtual void ComputeWeights()
{
    if (this.ObjectsToLoot == null)
    {
        return;
    }
    if (this.ObjectsToLoot.Count == 0)
    {
        return;
    }

    // Set a uniform weight for all objects
    float uniformWeight = 1f; // or any other value you'd like to use

    // Initialize maximum weight so far
    this._maximumWeightSoFar = 0f;

    foreach (T t in this.ObjectsToLoot)
    {
        // Assign the uniform weight to each object
        t.Weight = uniformWeight;

        // Calculate range for each object
        t.RangeFrom = this._maximumWeightSoFar;
        this._maximumWeightSoFar += t.Weight;
        t.RangeTo = this._maximumWeightSoFar;
    }

    // Store total weight
    this.WeightsTotal = this._maximumWeightSoFar;

    // Set chance percentage for each object (all will have the same percentage)
    foreach (T t2 in this.ObjectsToLoot)
    {
        t2.ChancePercentage = t2.Weight;
    }

    this._weightsComputed = true;
}
```

And it looked fine and made sense so i throw it into dnSpy, and try to complete level. After few clicks, flag spawned. Picking it up reveals `GHCTF{wow_wow_lucky_pull}`.

[Jump to next one](./Level-9.html)


