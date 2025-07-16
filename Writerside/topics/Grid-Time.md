# Grid Time
<secondary-label ref="gm-mod"/>

## Problem

When the `TW_MonitorPositions` aggregates the spawn points around the player(s), then randomly selects a spawn point from that pool.

The issue with this approach is if players congregate in a particular area, the AI will hit their cap. If a player decides 
to b-line it out of the area, the AI will not spawn around them. 

If AI die, they can potentially spawn near the other players clear across the area of operations nowhere near the player we're interested in.

## Proposal

As new grids become active, we should assign it the time it became active. We should prioritize grids that have recently become active.
The threshold for that can be configurable. 

If any grids meet this threshold filter, we should only include those grids in result sets. Otherwise, return things as we currently are.

This solution only fixes spawning "net new" AI. AI that have been garbaged collected or killed. The secondary issue to address in the future is 
reclaiming AI from areas - without breaking immersion. We'd have to remove AI that aren't in view and aren't alert.