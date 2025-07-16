# Spawn Density
<secondary-label ref="spawn-system"/>
<secondary-label ref="gm-mod"/>

## Problem

If you haven't noticed already, you could be strolling through the fields and end up with... 20 AI gaggled around a deer-stand. 

The current implementation uses the `TW_GridCoordArrayManager` which pools `TW_AISpawnPoint` into grid coordinates. When the spawn manager 
receives player positions from `TW_MonitorPositions` it simply aggregates all the spawn points from those coordinates.

When spawning AI, the system selects a random spawn point from the aggregated pool. Which, is why in more unpopulated areas 
you tend to get a gaggle of AI.

## Proposal

Refactor `TW_GridCoordArrayManager` to accept a filter class. When iterating over the grids, if a filter is set we can 
pass the coordinates through a filter to determine whether that coordinate gets included in the resultset.

```C
class TW_GridFilterBase
{
    bool ShouldInclude(int x, int y);
}
```

When the grid-manager is initialized, or size changes, we can calculate a _budget_ per grid.

```Markdown
Grid
    - number of spawns in grid (SpawnCount)
    - Max AI that can spawn in grid (AIBudget)

Configurable ratio to apply to grids (BudgetRatio).

For each grid, we need to normalize the maximum allowed spawns. 
Otherwise cities will have insane maximum budgets.
```

**Budget Ratio:** 0.2

<table>
    <tr>
        <td>Coord</td>
        <td># of Spawn Points</td>
        <td>AI Budget</td>
    </tr>
    <tr>
        <td>-1, 1</td>
        <td>10</td>
        <td>2</td>
    </tr>
    <tr>
        <td>-1, 0</td>
        <td>100</td>
        <td>20</td>
    </tr>
    <tr>
        <td>0, 0</td>
        <td>1000</td>
        <td>200</td>
    </tr>
</table>

The budget ratio is not enough. It's great for unpopulated areas, but areas that are more dense will have wild numbers.

To combat this we can use a separate configurable value **MaxAIPerGrid**. Then we can take the minimum value between the 
calculated budget and **MaxAIPerGrid**. 

If set to **10**, we can reign things in.

<table>
    <tr>
        <td>Coord</td>
        <td># of Spawn Points</td>
        <td>AI Budget</td>
        <td>Actual Budget</td>
    </tr>
    <tr>
        <td>-1, 1</td>
        <td>10</td>
        <td>2</td>
        <td>2</td>
    </tr>
    <tr>
        <td>-1, 0</td>
        <td>100</td>
        <td>20</td>
        <td>10</td>
    </tr>
    <tr>
        <td>0, 0</td>
        <td>1000</td>
        <td>200</td>
        <td>10</td>
    </tr>
</table>

When we spawn AI, we can associate the AI to the grid it spawned in. The AI will remain associated with the grid until
it dies. Even if it moves into other grids. This should make for some interesting gameplay.