# Add Loot

Our mod scrapes data from the Faction Manager. This means if you add a mod which does not have its contents within an Entity Catalog (which Faction Manager uses), 
you will not see that mods stuff!

So how would you add those items into the loot table you may ask?

----

## Add Custom Category for loot

This is the base game's Arsenal Item Type. We programmatically scrape these enum values to produce our categories. If you
add a new entry here via modding it should work.

```C
//~Update config: ArsenalItemTypeUIConfig.conf if Enum changes as well as any Editor related Attributes!
enum SCR_EArsenalItemType
{
	RIFLE = 1 << 1,
	PISTOL = 1 << 2,
	LETHAL_THROWABLE = 1 << 3,
	ROCKET_LAUNCHER = 1 << 4,
	MACHINE_GUN = 1 << 5,
	HEAL = 1 << 6,
	BACKPACK = 1 << 7,
	SNIPER_RIFLE = 1 << 8,
	NON_LETHAL_THROWABLE = 1 << 9,
	HEADWEAR = 1 << 10,
	TORSO = 1 << 11,
	VEST_AND_WAIST = 1 << 12,
	LEGS = 1 << 13,
	FOOTWEAR = 1 << 14,
	RADIO_BACKPACK = 1 << 15,
	EQUIPMENT = 1 << 16,
	WEAPON_ATTACHMENT = 1 << 17,
	EXPLOSIVES = 1 << 18,
	HANDWEAR = 1 << 19,
	MORTARS = 1 << 20,
	HELICOPTER = 1 << 21,
	VEHICLE = 1 << 22,
}
```

Example of adding your own category

```C
modded enum SCR_EArsenalItemType
{
    MY_CATEGORY = 1 << 50;
};
```

Example of Loot Component: 

![lootmap-component.png](lootmap-component.png)

<tip>
Notice the checkboxes on the right which correlate to categories. By default categories are turned off. So if you add a new 
category you'll need to find the prefabs you want to modify and ensure its enabled.
</tip>

```json
{
  "LootTable": {
    "MY_CATEGORY": [
      {
        "$type": "TW_LootConfigItem",
        "resourceName": "{F1E29085C62F0D55}Prefabs/Characters/Uniforms/Jacket_KZS/Jacket_KZS.et",
        "chanceToSpawn": 25,
        "randomSpawnCount": 1,
        "isEnabled": true,
        "tags": []
      }
    ]
  }
}
```