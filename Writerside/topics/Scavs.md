# Scavs

Originally part of our extraction mod (which has since been broken up into modular systems), the intent was to have AI that 
could spawn with random loot.

Our system is not hard-coded. In fact, the loadout given to Scavs is based on the lootmap! 

We introduced the `TW_RandomInventoryComponent` which can be attached to a character of your choosing. Once attached,
there's nothing further needed!

![random-inv-component.png](random-inv-component.png)

In the future, we'll look at spawn random loot without requiring the component.

<tip>
    The Scavs are added to the "character" pool for each vanilla faction. When paired with our 
    Spawn System mod, you can achieve interesting gameplay!
</tip>

<table>
    <tr>
        <td>spawnWithBackpackChance</td>
        <td>Chance the AI will be given a backpack</td>
        <td>0.1 (10% chance)</td>
    </tr>
    <tr>
        <td>spawnWithTwoWeaponsChance</td>
        <td>Chance the AI will be given two weapons</td>
        <td>0.1 (10% chance)</td>
    </tr>
    <tr>
        <td>spawnWithHealChance</td>
        <td>Chance the AI will be given healing items (whichever items are in the Heal category)</td>
        <td>0.25 (25% chance)</td>
    </tr>
    <tr>
        <td>spawnWithVestChance</td>
        <td>Chance the AI will be given a vest</td>
        <td>0.3 (30% chance)</td>
    </tr>
</table>