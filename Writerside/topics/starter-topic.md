# TrainWreck

<img src="trainwreck.jpg" alt="TrainWreck-Image"/>

<a href="2025-07-15.md">Latest Changes</a>

## Overview

TrainWreck mods try to be as configurable as possible, allowing folks to tweak things to their 
liking without having to create a new mod.

<code-block lang="bash">
$profile:lootmap.json
$profile:spawnSettings.json
</code-block>

### GM Mod

Contains centralized logic that's shared between other TrainWreck mods. 

Was the stop-gap solution in providing our Game Master tools they needed for spawning AI in buildings.

<a href="https://reforger.armaplatform.com/workshop/61F8FB543ECB6949-TrainWreck-GM">Reforger Workshop</a>

### Spawn System Mod

Dynamically spawns AI around player positions based on configurable values. This system dynamically loads factions from the
`SCR_FactionManager`, allowing it to work with other mods.

<a href="https://reforger.armaplatform.com/workshop/627912F23F2FC55C-TrainWreck-SpawnSystem">Reforger Workshop</a>

### Loot System Mod

Dynamically pulls from `SCR_FactionManager`'s entity catalog to populate a `$profile:lootmap.json` file. This system will 
merge with what it has with what it finds at run-time. Allowing you to hot-swap mods with ease. 

If a mod is not loaded, the system will skip said mod to avoid breaking.

<a href="https://reforger.armaplatform.com/workshop/626E39840DDC4323-TrainWreckLooting">Reforger Workshop</a>

<seealso title="Mods">
    <category ref="related">
        <a href="Spawn-in-Buildings.md">GM Mod</a>    
        <a href="Overview.md">Looting Mod</a>
    </category>
</seealso>
