---
icon: material/store
---

# Owned Shops

Player-owned shops, plus configurable normal (NPC) shops with stock and pricing controls.

---

!!! danger "Do not delete `ignore.json`"
    The `ignore.json` file in the `JSON/` folder must remain present at all times.

## :material-cog: Config

=== "QBCore"

    Default config — no changes needed.

=== "ESX"

    ```lua
    Config.Core             = "esx"                                    -- "esx" or "qbcore"
    Config.CoreFolderName   = "es_extended"                            -- es_extended || qb-core
    Config.PlayerLoadedEvent = "esx:playerLoaded"                      -- esx:playerLoaded || QBCore:Client:OnPlayerLoaded
    Config.PlayerUnloadEvent = "esx:onPlayerLogout"                    -- esx:onPlayerLogout || QBCore:Client:OnPlayerUnload
    Config.Inventory        = "ox"                                     -- "qb" or "ox" or "qs"
    Config.Target           = "qtarget"                                -- "qtarget" — has backward compat with ox_target / qb-target
    ```

## :material-package-variant: Inventory

For ox_inventory, set `Config.Inventory = "ox"` and update `Config.ImagePath`:

```lua
Config.Inventory = "ox"
Config.ImagePath = "nui://ox_inventory/web/images/"
```

## :material-store-marker: Normal shops

Normal shops have **unlimited stock** — money paid here disappears (sink). You can configure as many categories as you want with item names and prices.

!!! tip "Customisable peds"
    The ped spawning and scenario logic in `cl_normalshops.lua` is **completely open** — edit it however you like if you know what you're doing.

```lua
["247"] = {                                              -- unique shop identifier (must be unique)
    name = "24/7 Shop",                                  -- displayed at the top of the shop UI
    blip = {                                             -- map blip
        id = 59, colour = 69, scale = 0.8
    },
    items = {                                            -- items + price for this shop
        { name = "phone", price = 100 }
    },
    locations = {                                        -- shop locations
        -- coords   = vector4
        -- ped      = ped model
        -- scenario = ped scenario
        { coords = vector4(230.58, -878.64, 30.49, 107.01), ped = "a_f_m_tourist_01", scenario = "WORLD_HUMAN_GUARD_STAND" },
    }
},
```

## :material-account-key: Permissions

Add identifiers for players allowed to use the menu to create owned shops:

```lua
Config.Permissions = {
    ["license:6d3b6254a50416697dcaa91878e2eb03d9112302"] = true,
    ["fivem:1234"]  = true,
    ["steam:1234"]  = true,
}
```
