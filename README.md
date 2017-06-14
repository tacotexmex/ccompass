# ccompass

This minetest mod adds a calibratable compass to the minetest.

  - License - as the original mod (https://forum.minetest.net/viewtopic.php?f=11&t=3785): Code: WTFPL, textures: CC BY-SA
  - Dependencies to other mods: none
  - Forum: https://forum.minetest.net/viewtopic.php?f=9&t=17881
  
## For Players:
 <TODO: nice screenshot> 
 
1. Craft the compass as before using the crafting recipe.
   The new compass points to the origin / Zero point and is already usable.
2. Punch the compass to a compatible node (all by default) and enter the target name
3. Now this compass leads you allways to this location. You can give it away to allow other users to find this place.


## For server owners:
The mod support the next settings:

    static_spawnpoint - use this position instead 0,0,0 as initial target
    ccompass_recalibrate (enabled by default): If disabled each compass can be calibrated one time only
    ccompass_restrict_target (Disabled by default): If enabled, only specific nodes are allowed for calibration (usable with any type waypoint nodes for example)
    ccompass_restrict_target_nodes: List of technical node names allowed for compass calibration, separated by ','
    ccompass_aliasses: If enabled the compas:* items will be aliased to the ccompass:* items for compatibility


##  For developers:
1. It is possible to change compass settings from other mods by changing values in global table ccompass. So it is possible for example to add a waypoint node to the target-nodes by

```
    ccompass.restrict_target_nodes[nodename] = true
```

2. The pointed node metadata will be checked for "waypoint_name" attribute. It this attribute is set, the calibration screen take this string as proposal. This can be used for a game specific calibration node. To get it working working just set in node definition something like

```
          after_place_node = function(pos, placer)
             local meta = minetest.get_meta(pos)
             meta:set_string("waypoint_name", "the unique and wunderfull place")
          end,
```

3. It is possible to create pre-calibrated compasses trough other mods. Just write the position to the Itemstack meta:

```
    local stack = ItemStack("ccompass:0")
    stack:get_meta():set_string("target_pos", minetest.pos_to_string(pos))
```


4. Each time the compass is updated, a hook is called, if defined in other mod. The hook is usefull to implement wear or any other compass manipulation logic.
```
    function ccompass.usage_hook(compass_stack, player) 
        --do anything with compasS_stack
        return modified_compass_stack
    end
```
