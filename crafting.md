# Dependencies: 
- ox_inventory
- QB/QBX/Esx

# Commands
- /crafting:admin to open admin crafting options

# Installation
- Go through the shared folder properly. There are comments on every line explaining what each thing does.
- Setup your webhooks in server/open/sv_webhooks.lua
- Make sure the script starts at the very end in your server.cfg so all the dependencies start before the crafting script.
- Database tables are installed automatically so no need to run any sql file.
- All the ui locales are present in html/locales
- Install the following item. (No image required. Images are configured for blueprints through shared/recipes.lua)
```lua
["crafting_blueprints"] = {
    label = "Blueprints",
    weight = 10,
    stack = false,
},
```

# Install Items To Ox Inventory and copy Images (Only required if you use snipe-motel)
- Install the following items
- All the images are present in snipe-crafting/images folder
- If you use snipe-motel, make sure the script starts after snipe-motel. (And also after the framework/ox_lib/target etc). Best option is adding it at the end of the list in your server.cfg

```lua
["crafting_table_meth"] = {
    label = "Crafting Table (Meth)",
    weight = 0,
    stack = true,
    close = true,
    consume = 0,
    description = "Crafting Table",
    server = {
        export = "snipe-crafting.placeCraftingTable"
    }
},

["crafting_table_saw"] = {
    label = "Crafting Table (Saw)",
    weight = 0,
    stack = true,
    close = true,
    consume = 0,
    description = "Crafting Table",
    server = {
        export = "snipe-crafting.placeCraftingTable"
    }
},

["crafting_table_tool"] = {
    label = "Crafting Table (Tool)",
    weight = 0,
    stack = true,
    close = true,
    consume = 0,
    description = "Crafting Table",
    server = {
        export = "snipe-crafting.placeCraftingTable"
    }
},

["crafting_table_weapon"] = {
    label = "Crafting Table (Weapon)",
    weight = 0,
    stack = true,
    close = true,
    consume = 0,
    description = "Crafting Table",
    server = {
        export = "snipe-crafting.placeCraftingTable"
    }
},

["crafting_table_bigtool"] = {
    label = "Crafting Table (Big Tool)",
    weight = 0,
    stack = true,
    close = true,
    consume = 0,
    description = "Crafting Table",
    server = {
        export = "snipe-crafting.placeCraftingTable"
    }
},
```

# Exports

## Server Side
```lua
-- to spawn a blueprint for a specific item with limit on how many items can be crafted with that blueprint (Make sure the item is configured in recipes.lua)
exports["snipe-crafting"]:SpawnSpecificBlueprint(source, item, limit) 

--to spawn *howmany* blueprints. The blueprints are spawned randomly and depends on the rarity of the blueprint added in recipes.lua. Make sure you have blueprints for every type otherwise it wont give blueprints sometimes
exports["snipe-crafting"]:SpawnRandomBlueprints(source, howmany) 
```

Eg.

```lua
exports["snipe-crafting"]:SpawnSpecificBlueprint(source, "weapon_pistol", 5) -- will spawn blueprint for pistol with limit of 5

exports["snipe-crafting"]:SpawnRandomBlueprints(source, 5)  -- will spawn 5 random blueprints present in recipes.lua
```