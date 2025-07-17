# Location Settings
<secondary-label ref="code-complete"/>
<secondary-label ref="not-used"/>

Reforger has various location types, from military bases, harbors, villages, hills, etc. 
The map renders these in various text formats! 

In Arma 3, when we wanted to query the various locations, we'd use the `configFile`! We don't have
that option in Reforger. Instead, we can query the map via `EMapDescriptorType` [Reforger Reference](https://arexplorer.zeroy.com/_e_map_descriptor_type_8c.html)!

Since we want everything to be configurable, we exposed the following:

```JSON
{
  "isEnabled": true,
  "gridSize": 150,
  "locationSettings": {
    "MDT_NAME_VILLAGE": {
      "radius": 4
    },
    "MDT_NAME_HILL": {
      "radius": 2
    }
  }
}
```

Similar to the Spawn System mod, the location settings is a dictionary
where you can assign the radius in chunks around a location type that will be considered "part of that location".

For instance, a radius of 4 for a village effectively means an 8x8 grid. The center would be wherever the 
descriptor is located on the map.

In our opinion, this makes it easier to work with modded maps! So long as map designers follow the same standard of using 
`EMapDescriptorType`. 