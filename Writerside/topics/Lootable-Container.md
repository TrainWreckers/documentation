# Lootable Container

How does the Looting system recognize something as lootable?

- A handful of Inventory Component must be present on the prefab in ordre for it to be considered an "inventory".
- Our `TW_LootableInventoryComponent` must be added

## How to make something lootable

<procedure title="Setup">
    <step>
        Navigate to the prefab you want to make lootable
    </step>
    <step>
        Right click --> Override
    </step>
</procedure>

### RplComponent

If this component does not exist, add it.

If this component <strong>does</strong> exist, but is disabled - enable it.

### ActionsManagerComponent

![ActionsManagerComponent](actions-manager.png)

<procedure title="How to setup ActionsManagerComponent">
    <step>
        Add at least one <strong>Action Context</strong>. This sets the positional/conditional context in how an action is presented to the player. When naming the
        context we have been using "default" for simplicity.
    </step>
    <step>Within the additional actions, make sure to select the previously created (or existing) context</step>
    <step>The UI Info class can be set, but the values here don't matter</step>
    <step>The position, for most cases can be a <strong>PointInfo</strong> class. Click on the offset value in the property editor. 
            This should allow you to move the transform around in the editor. Note how the x/y/z values will change as you move the transform around.
    </step>
    <step>
        <p>Update the radius to something reasonable. We found 0.8 to be a sweet spot for most things.</p>
        <p>For further assistance, you can right-click on the <strong>ActionsManagerComponent</strong> to enable debug drawing</p>
        <img src="gizmo-drawing.png" alt="gizmo"/>
        <img src="showcase-gizmo.png" alt="showcase gizmo"/>
    </step>
    <step>Add an "Additional Actions", set it to <strong>SCR_OpenStorageAction</strong></step>
    <step>Set the parent context to "default", or whichever one you want from the <strong>Action Contexts</strong> list</step>
    <step>The UI Info, again, can be set but the values here don't matter</step>
</procedure>

### SCR_InventoryStorageManagerComponent

This is an easy step. Just make sure it's present, and enabled. If it's not present add it! Done!

### SCR_UniversalInventoryStorageComponent

![Universal Inventory Storage Component](universal-storage-manager-component.png)

<procedure>
    <step>Set "Attribute" to <strong>SCR_ItemAttributeCollection</strong></step>
    <step>Set "Item Display Name" to something reasonable. This is what appears to the player when they're in the inventory screen</step>
    <step>
        <p>Now we're not entirely sure if this impacts things but the following size types are what we've been using</p>
        <p>"Size" --> <strong>Slot 3x3</strong></p>
        <p>"Slot Type" --> <strong>Slot_Any</strong></p>
    </step>
    <step>Use Capacity Coefficient --> Disable it</step>
    <step>Max Cumulative Volume --> 10000</step>
    <step>Max Weight --> 10000</step>
</procedure>

### TW_LootableInventoryComponent
This should be straight forward. What kind of loot do you want to spawn in here? Check the boxes to allow certain types to spawn.

<tip>
    We have not implemented or used "Loot Item Modes" yet. So setting this value will not impact anything.
</tip>