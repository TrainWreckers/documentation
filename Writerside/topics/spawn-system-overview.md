# Overview
<secondary-label ref="spawn-system"/>

The Spawn System scrapes info from the `SCR_FactionManager`. Then it saves it to `$profile:spawnSettings.json`.
If this file already exists, it will load it from disk.

Currently, we save **Characters**, **Vehicles**, and **Groups** from each faction. 
[Looting mod](Overview.md) saves the **Items** into a separate file (`$profile:lootmap.json`).

This mod was designed from the ground up to be configurable. You can tweak which factions are enabled, disabled, 
how many AI to spawn along with the weights for which character prefabs to use.


## Example Behavior

```json
{
  "FactionName": "US",
  "ChanceToSpawn": 10,
  "MaxAmount": 20,
  "IsEnabled": true,
  "AIWanderChance": 0.25,
  "Characters": [
    {
      "PrefabName": "{5EFC243926EE6808}Prefabs/Characters/Factions/BLUFOR/US_Army/Character_US_Randomized.et",
      "Chance": 27
    },
    {
      "PrefabName": "{26A9756790131354}Prefabs/Characters/Factions/BLUFOR/US_Army/Character_US_Rifleman.et",
      "Chance": 0
    }
  ]
}
```

- Since the US faction is enabled, the system will overtime spawn AI until it meets the **MaxAmount**. Overtime, if AI from this faction dies, it'll spawn more - keeping the AI levels greater than or equal to **MaxAmount**.
- There is a 25% chance AI spawned for this faction will be tagged as "wanderers" which are units that get sent to random points within the area of operations.
- Based on the two character types, only the randomized prefab will be used (because the other prefab has a chance of 0).
- Chance to spawn behaves like a weight. Factions with higher weights are prioritized when spawning units.