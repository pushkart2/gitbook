# Important 

## DO NOT DELETE `ignore.json` file in JSON folder
# Config

if you use ESX, make sure to change the config to look like this:

```lua 
Config.Core = "esx" -- "esx" or "qbcore"

Config.CoreFolderName = "es_extended"  -- es_extended || qb-core

Config.PlayerLoadedEvent = "esx:playerLoaded" -- esx:playerLoaded || QBCore:Client:OnPlayerLoaded
Config.PlayerUnloadEvent = "esx:onPlayerLogout" -- esx:onPlayerLogout || QBCore:Client:OnPlayerUnload     

Config.Inventory = "ox" -- "qb" or "ox" or "qs" -- if you use ox_inventory

Config.Target = "qtarget" -- "qtarget" -- if you ox_target, just select qb-target or qtarget (it has backwards compatibility)
```

# Inventory

If you use ox_inventory, make sure to set the inventory to "ox" and also change the ImagePath

```lua
Config.Inventory = "ox"
Config.ImagePath = "nui://ox_inventory/web/images/"
```

# Normal Shops

- These will be normal shops which will have unlimited items and the money removed from these shops will go to black hole. You can set as manu categories as you want and set the item name and price for each item.
- The code to spawn the ped and their scenario here are completely open in cl_normalshops. You can edit it to your liking if you know what you are doing. 
```lua
 ["247"] = { -- unique identifier for the shop (make sure these are unique)
        name = "24/7 Shop", -- label that will show on top of the ui of shop
        blip = { -- blip related info for the shops
			id = 59, colour = 69, scale = 0.8
		},

        items = { -- item name and price for the item that will be available at this shop
            {name = "phone", price = 100}
        },
        locations = { -- locations for this particular shop
        -- coords = vector4
        -- ped = ped Model
        -- scenario = scenario for the ped
            {coords = vector4(230.58, -878.64, 30.49, 107.01), ped = "a_f_m_tourist_01", scenario = "WORLD_HUMAN_GUARD_STAND"},

        }
    },
```

# Permissions
- In Config.lua, you can add the identifiers for players who will be allowed to use the menu to create owned shops.
```lua
Config.Permissions = {
    ["license:6d3b6254a50416697dcaa91878e2eb03d9112302"] = true, -- this is my license, you can add your own
    ["fivem:1234"] = true,
    ["steam:1234"] = true,
}
```
