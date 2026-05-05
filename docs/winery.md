---
icon: material/glass-wine
---

# Winery

Grow grapes, ferment wine, and run a full winery business.

---

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
    Config.ProgressBar      = "ox"                                     -- "qb" or "ox"
    ```

## :material-tune: Config parameters

- Read every comment in `config.lua` — every variable is documented inline.
- Inventory and progress-bar code is **fully open** and editable. If a locale appears to be missing in config, it lives in an unencrypted file.

!!! tip "Adding new wines (ox_inventory)"
    To add wines like `redwine` or `whitewine`, define them in **both** `config.lua` and `ox_inventory/items.lua` (use the existing `whitewine` entry as a template). Make sure the `server.export` field is copied so the use logic triggers correctly.
