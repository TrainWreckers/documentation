# TW_MonitorPositions

## Overview

Knowing where players are is critical for various reasons. Where should things spawn? Where shouldn't things spawn?

The GM-Mod introduces a grid-system that allows `TW_AISpawnPoints` and `TW_VehicleSpawnPoints` to be grouped by grid coordinates. 

This enables us to grab upwards of 10s, 100s, 1000s, of spawn points by querying a single coordinate! To grab additional points, it's a matter of x, or y +/- N then combining the results.

## Grid Updates

At a regular interval, which the `TrainWreck-SpawnSystem` overrides and makes configurable - player positions are checked. Then we iterate over each radius, sending the calculated 
player chunks to subscribers.

## Subscribe

The `TrainWreck-SpawnSystem` mod has two types of positions it cares about. Where to spawn AI and where NOT to spawn AI. Previously, we tried combining these two types. However, we have since 
separated it. Now each "type" requires you to subscribe to it with a unique name.

So for the spawn system example, we'd have two systems subscribed to two "radiuses", or potentially the same radius! 

Registration example

```C
// 500 -> Grid Size
// 3 -> Radius (in chunks) around a player
ref TW_OnPlayerPositionsChangedInvoker subscription = TW_MonitorPositions.GetInstance().AddGridSubscription("MySystemName", 500, 3);
subscription.Insert(OnPlayerPositionChanged);
```

Example of the update event

```C
private ref set<string> m_PlayerChunks = new set<string>>();

void OnPlayerPositionChanged(GridUpdateEvent gridUpdate)
{
  m_PlayerChunks.Clear();
  m_PlayerChunks.Copy(gridUpdate.GetPlayerChunks());
}

// Our spawn system has a function which executes at an interval and leverages the data from from this function
```

<tip>
    When you add or remove a subscription we return the <strong>TW_OnPlayerPositionsChangedInvoker</strong> to allow you to add your callback function, or to remove it.
</tip>