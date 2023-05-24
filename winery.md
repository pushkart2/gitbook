# Config

if you use ESX, make sure to change the config to look like this:

```lua 
Config.Core = "esx" -- "esx" or "qbcore"

Config.CoreFolderName = "es_extended"  -- es_extended || qb-core

Config.PlayerLoadedEvent = "esx:playerLoaded" -- esx:playerLoaded || QBCore:Client:OnPlayerLoaded
Config.PlayerUnloadEvent = "esx:onPlayerLogout" -- esx:onPlayerLogout || QBCore:Client:OnPlayerUnload     

Config.Inventory = "ox" -- "qb" or "ox" or "qs" -- if you use ox_inventory

Config.Target = "qtarget" -- "qtarget" -- if you ox_target, just select qb-target or qtarget (it has backwards compatibility)

Config.ProgressBar = "ox" -- qb | ox
```

# Config Parameters
- Go through the config properly to see comments near each variable to see what it does
- All of the inventory, progressbar code is completely open and editable. If the locales are missing in config, they are probably in an unencrypted file.
- If you ox_inventory, and want to add new wines like redwine and whitewine, you add them in config.lua and also in ox_inventory items list like I have done for whitewine. Make sure the export and everything is copied over probably so they are useable