# Add Loot

Our mod scrapes data from the Faction Manager. This means if you add a mod which does not have its contents within an Entity Catalog (which Faction Manager uses), 
you will not see that mods stuff!

## Knowledge Share

What is an **EntityCatalog**? This is basically a collection of things that is used to describe various aspects about a Faction. 

- The character prefabs for the faction
- The vehicle prefabs for the faction
- The group prefabs for the faction
- The inventory items for the faction

In particular, we're interested in the **inventory items**.

![entity-catalog-with-arsenaldata.png](entity-catalog-with-arsenaldata.png)

Notice the breakdown of items within the catalog. Weapons, ammunition, etc. If you were to expand these categories and look
at an item you'll see data associated with each item.

For example, the Smoke Grenade has two "Entity Data" objects. **ResupplyData** and **Arsenal Data**. We're specifically interested 
in the **Arsenal Data**. As the screenshot depicts, the smoke grenade is linked to **NON_LETHAL_THROWABLE**. For our purposes, this 
is good enough to associate a prefab to a category!

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
<warning>
Note: The <strong>SCR_EArsenalItemType</strong> snippet is verbatim from reforger vanilla. The <strong>ArsenalItemTypeUIConfig.conf</strong> is what 
you end up seeing as GameMaster when tweaking what spawns within an Arsenal. <br/>
You aren't <strong>required</strong> to update this file, unless you want to play nice with Arsenals.
</warning>

Example of adding your own category

```C
modded enum SCR_EArsenalItemType
{
    MY_CATEGORY = 1 << 50;
};
```

## Ensure loot shows up

Example of Loot Component: 

![lootmap-component.png](lootmap-component.png)

<tip>
Notice the checkboxes on the right which correlate to categories. By default, categories are turned off. So if you add a new 
category you'll need to find the prefabs you want to modify and ensure its enabled.
</tip>

If, for example, you want to change what spawns in a fridge, you would find the fridge prefab and override it.

Likely, the prefab will already be overriden within our mod (if from vanilla), so your mod will want to override ours. Otherwise, if it's not in our mod you can override
that prefab directly. 

<note>
The reason we say you should override our mod's version of a prefab if it exists is to prevent conflicts. If we modify the same 
component(s) - reforger can have a hard time resolving the differences and break the prefab entirely.
</note>

### How to locate prefab

In the world you can right-click on an object and locate the prefab (to help find it faster), or you can search within the resource browser.

![locate-prefab.png](locate-prefab.png)

![override-fridge.png](override-fridge.png)

![override-fridge-2.png](override-fridge-2.png)

If the prefab you are overriding is not something we have modified, please refer to [how to make sure that prefab is lootable](Lootable-Container.md)

## Entity Catalog

As previously mentioned, we scrape data from the **FactionManager**. Other mods such as **Overthrow** does the same thing. 

If the prefab(s) you want to be lootable are **not** in one of the faction entity catalogs the item **will not appear**. So long as 
at least one faction has the prefab, our lootmap will pick it up.

Worth noting that we remove duplicate entries. Whatever appears first... that's how we do it. FIA and USSR both have SVD snipers in their catalogs. 
Whichever faction appears first (FIA) ends up being the prefab we grab. For a visual (`{3EB02CDAD5F23C82}Prefabs/Weapons/Rifles/SVD/Rifle_SVD.et`), 
we are referring to the full resource name of a prefab. So you aren't missing or losing anything by us not having duplicates.

The kicker here is if you intended on having the item spawn within multiple categories. You can either add it manually in our json (not fun) or create a new category (as described by this guide) then enable 
said category on the prefabs you wish to have said item spawn in. We are following vanilla intent here.

### How do I know which catalog to modify?

Ideally, you should have a scenario type in mind. Load the scenario up within the Workbench then find the **Faction Manager** - which you can filter for.

![faction-manager-search.png](faction-manager-search.png)

There will be a list of factions in the manager. If there is blue-text on a field it means it's using a `.conf` file. Which you can right-click and navigate towards.

Which faction you modify is entirely up to you. If your goal is to allow prefabs to spawn within each faction's arsenal then you will have to add your item to _each_ faction's catalog.
If your goal is just to have the loot appear (don't care about arsenal), then adding your prefabs to one faction is enough.

![list-of-factions.png](list-of-factions.png)

![catalog-direct-modify.png](catalog-direct-modify.png)

----

At this point, if you have followed everything you should be able to run your scenario and see the prefab(s) added to the lootmap.json.

Our mod will merge with whatever is active in the game, so your new stuff _should_ appear without you having to add it by hand! 

<warning>
Do not recommend adding prefabs by hand into lootmap.json. <br/><br/>
You can, if you're running your own server but note if you delete that file for whatever reason you will lose your custom stuff. <br/><br/>
If you want to distribute the mod and have those prefabs automatically appear - this guide should help you do that. If it doesn't please open a ticket on our <a href="https://github.com/trainwreckers/trainwrecklooting/issues">GitHub</a>
</warning>

**Example of lootmap.json** (reduced the scope for brevity)

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