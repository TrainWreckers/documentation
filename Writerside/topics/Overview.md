# Overview

The Loot system both pulls from a JSON file `$profile:lootmap.json`, but scrapes and merges catalog info from the active 
`SCR_FactionManager`. This enables us to work with any mod! 

Everything about our mod is configurable, allowing you to tailor the experience to your community.

<tip>
    You can swap mods out at anytime without regenerating the lootmap.json. Each resource is validated prior to 
    being added to the loot pool.
</tip>

## Ammo

Finding magazines that are full, ALL THE TIME, pshh. If you want to make ammo scarce, this is how you scare players! 

By default we spawn magazines between 80% - 100% capacity. 

```JSON
{
  "AmmoPercentageSetting": {
    "$type": "PercentageFieldSetting",
    "Min": 80,
    "Max": 100
  }
}
```

<warning>
    Should pair this mod with a mag repacking mod.
</warning>

Skamdrept and myself (Badger 2-3) worked on SESOF MagRepack. ACE pulled in our work to include with their series of mods. 
Which is pretty neat!

- [SESOF Mag Repack](https://reforger.armaplatform.com/workshop/5ED143120C5465BD-SESOFMagRepack)
- [ACE Magazine Repack](https://reforger.armaplatform.com/workshop/611CB1D409001EB0-ACEMagazineRepack)

## Loot Respawn

If enabled, loot can respawn overtime so long as players are not nearby, and the container hasn't been interacted with 
for a period of time. 

<table>
    <tr>
        <td>Variable</td>
        <td>Description</td>
        <td>Default Value</td>
    </tr>
    <tr>
        <td>IsLootRespawnable</td>
        <td>Should loot be allowed to respawn</td>
        <td>true</td>
    </tr>
    <tr>
        <td>RespawnAfterLastInteractionInMinutes</td>
        <td>
            <p>After being searched, a container must not be interacted with by <strong>ANY</strong> player for this period of time
            for it to be eligible for loot respawn.</p>
            <p>Note, in order for loot to respawn players must also be outside a certain radius as defined by <strong>DeadZoneRadius</strong></p>
        </td>
        <td>1</td>
    </tr>
    <tr>
        <td>RespawnLootTimerInSeconds</td>
        <td>Frequency in which containers are checked whether they can respawn loot</td>
        <td>10</td>
    </tr>
    <tr>
        <td>RespawnLootRadius</td>
        <td>Number of chunks around a player loot will respawn. Note: this number should be greater than <strong>DeadZoneRadius</strong></td>
        <td>5</td>
    </tr>
    <tr>
        <td>DeadZoneRadius</td>
        <td>Number of chunks around a player loot <strong>CANNOT</strong> respawn. 
            Players must be outside this area for a container to be eligible to respawn.
            The intent is for this to prevent players from camping good-loot containers.
        </td>
        <td>
            2
        </td>
    </tr>
    <tr>
        <td>GridSize</td>
        <td>Loot Containers are grouped together on a grid system. Their positions are divided by this value which becomes their coordinate.</td>
        <td>100</td>
    </tr>
    <tr>
        <td>UnlootedTimeRatio</td>
        <td>
            <p>We have a ratio we apply towards the <strong>EntitySize</strong> to determine how long it takes to "search" a container.</p>
            <p>Players must complete this interaction in order for loot to spawn in the container. Cannot bypass via inventory menu.</p>
            <p>Zero means instantly open, increase this value for it to take longer to open.</p>
        </td>
        <td>0.5</td>
    </tr>
    <tr>
        <td>NumberOfItemsToSpawnPerContainer</td>
        <td>
            <p>Number of items to spawn when a player completes searching.</p>
            <p>We may turn this into a min/max scenario, taking <strong>EntitySize</strong> into account, similar to <strong>UnlootedTimeRatio</strong></p>
        </td>
        <td>4</td>
    </tr>

</table>

## Loot Table

Vanilla reforger has the `SCR_EArsenalItemType` enum which pretty much behaves as a "category". We do NOT hardcode the implementation of these
values. So if modders want to expand upon this enum our mod will automatically incorporate it in our loot table. However, each container has
pre-defined categories defined for what can spawn in it.

TrainWreck adds a custom LootComponent to containers. <strong>IronMist</strong> used his methods of reasoning to determine what
could feasibly spawn in each container (categorically). So, even though we technically incorporate any category derived from `SCR_EArsenalItemType`, 
the containers would need to be updated to include any net-new categories.

```JSON
[
    {
      "$type": "TW_LootConfigItem",
      "resourceName": "{F1E29085C62F0D55}Prefabs/Characters/Uniforms/Jacket_KZS/Jacket_KZS.et",
      "chanceToSpawn": 25,
      "randomSpawnCount": 1,
      "isEnabled": true,
      "tags": []
    }
]
```

<table>
    <tr>
        <td>resourceName</td>
        <td>Resource path to prefab</td>
        <td><strong>DO NOT TOUCH</strong>. This value is automatically set by our system. If you modify this value and it's incorrect, this item will be ignored</td>
    </tr>
    <tr>
        <td>chanceToSpawn</td>
        <td>It's actually a weight system. The greater the number, the more likely it'll spawn</td>
        <td>When a new item is registered in the loot table it's given an automatic weight of <strong>25</strong></td>
    </tr>
    <tr>
        <td>randomSpawnCount</td>
        <td>
            <p>If this item is selected for spawn, it will select a random number between 1 and this value.</p>
            <p>It will then spawn N number of items into the designated container</p>
        </td>
        <td>Defaults to <strong>1</strong></td>
    </tr>
    <tr>
        <td>isEnabled</td>
        <td>If disabled, this item will be skipped/ignored</td>
        <td>true</td>
    </tr>
    <tr>
        <td>tags</td>
        <td>
            <p>Not yet integrated</p>
            <p>
                The intent is to integrate this with the map location system we've been developing. Where
                we can define areas around map location types to assign "tags" to things.
            </p>
            <p>
                For instance, we can say the area around military base markers - assign the tag "Military". Then we can filter
                loot from the loot table to only include items with said tag then pull from there
            </p>
        </td>
        <td>Empty list</td>
    </tr>
</table>
    