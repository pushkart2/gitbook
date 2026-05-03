# Step 1 (Config)

-- Read the comments in config and only edit the necessary things

# Step 2 (Compatibility)

## Compatibility
- Added compatibiltiy for multiple paid scripts. Please read the comments in config_compatibilty.lua and make the changes accordingly.

# Step 3 (Permissions)
## Permissions
- In Config folder check `permissions.lua`, there are options to set if the particular role for god, admin or mod can access all the panels.

All the roles here can access the admin menu
Only the GOD can set the panels for the other roles
There are unlimited options
1. God -> can access all the commands ( DO NOT REMOVE THIS ROLE )
2. Others can access panels that the god selects for them

If you have a new role, you can add it here and select to give either it any value you want and it will be available in the settings tab for god roles to select the panels for them.

eg. `["new_role"] = "God",`

eg. `["dev"] = "Admin",`

```lua
Config.GodRoles = {
    ["god"] = "God", 
    ["admin"] = "Admin",
    ["mod"] = "Moderator",
    -- if you want to add more roles just add them here
}
```


# Step 4 (Inventory)

## QB Inventory/ lj inventory

- Add the two following events at the end of the inventory/client/main.lua (do not add it at the beginning please!!)

```lua
RegisterNetEvent('inventory:client:SetCurrentTrunk', function(vehicle)
    CurrentVehicle = vehicle
end)

RegisterNetEvent('inventory:client:SetCurrentGlovebox', function(vehicle)
    CurrentGlovebox = vehicle
end)
```

# Step 5 (Bans (Only for ESX))

- Run the `bans.sql` file given in sql folder

# Step 6 (QB weathersync)

- if you use qb-weathersync, make the following changes in qb-weathersync/server/main.lua
```lua
local function isAllowedToChange(src)
    if src == 0 or QBCore.Functions.HasPermission(src, "admin") or IsPlayerAceAllowed(src, 'command') or exports["snipe-menu"]:isAdmin(src) then
        return true
    end
    return false
end
```

Replace this function.

# Step 7 Only for Illenium Appearance And ESX

Add this following block of code in illenium-appearance/client/framework/esx/compatibility.lua
```lua
RegisterNetEvent("snipe-menu:client:openAppearance", function()
    local config = GetDefaultConfig()
    config.ped = true
    config.headBlend = true
    config.faceFeatures = true
    config.headOverlays = true
    config.components = true
    config.props = true
    config.tattoos = true
    OpenShop(config, true, "all")
end)
```

# Step 8 (Admin Duty)
- With version 3.5.0+, I have implemented a new feature called Admin Duty. This will allow you to go on duty and off duty as an admin.
- Check config/config_new.lua for the Config Options
- Check config/permissions.lua for the permissions
- You need to do /adminduty if you want to use the admin menu. If you are not on duty, you will not be able to use the admin menu.
- Webhooks for when people go on duty and off duty are also available. You can change the webhook in server/open/sv_webhooks.lua

# Step 9 (Optional)
## How To add new UI Locales
- Please read each step properly. 
- To create a new locale, for eg. in language spanish, create a new file in html/locales called for eg. sp.json
- Copy the contents from en.json and put it in sp.json as is. Then modify it for your language. 
- Do not change the left side values, only the right side.
```json
"Revive": "Reanimar",
```
- Save the file and go to config.json in html folder and change lang to `sp` since your file is called `sp.json`
- Do not restart the script right away. Type `refresh` in server console first so it loads the newly created files and press enter and then do `ensure snipe-menu`.

## Keybinds
- Before starting the script on live server, go through cl_keybinds.lua file and see if you want to change the keybinds. Dont worry if you didnt configure, you can change them through settings. Go to GTA 5 Settings -> Keybinds -> Fivem and look for snipe-menu and change them.
- I would advise using mouse key for delete laser because it will be easier to access multiple keys.
- Keybinds only work if the dev mode is on. It can be turned on using the terminal button above the settings and only accessible to GodRoles.

## Discord Logging
- Change the webhook in sv_webhooks.lua. It logs almost each and every command used and who it was used on. There is not a lot of language options since it will be logged to a discord but you can change some things to your liking in config.

## Miscellanuous
- There are bunch of client/server scripts which are unencrypted which you can edit it to your liking. 
- I wont be adding supports for other paid scripts but the logic required to edit will be made open if needed.

# Step 10 (Optional Exports)
## Dev Mode Exports
- You are provided with two exports, one on client and one server to check if a person is in dev mode
- Usage (Client)
```lua
exports["snipe-menu"]:isDevMode()
```
- Usage (Server). Here source is the source/playerid for player
```lua
exports["snipe-menu"]:isDevMode(source)
```

## Admin Perms Exports
- You are provided with two exports, one on client and one server to check if a person has Admin perms
- Usage (Client)
```lua
exports["snipe-menu"]:isAdmin()
```
- Usage (Server). Here source is the source/playerid for player
```lua
exports["snipe-menu"]:isAdmin(source)
```

## Spectating
```lua
exports["snipe-menu"]:isInSpectating()
```

## Admin Role Name

- Returns the role name for player
- Usage (Server). Here source is the source/playerid for player
```lua
exports["snipe-menu"]:GetAdminRoleName(source)
```

# Developer Options

- You can create your own custom panels and add them to the menu. 
- All the things you need to make changes are in custom folder
- Check custom_config.lua for all possible options.
- Available Options for types on components inside a panel are: 

1. "string-input" (Input box for string)
2. "number-input" (Input box for number)
3. "checkbox" (Checkbox)
4. "regular-dropdown" (Dropdown with multiple options inside a table). Eg. {"Option 1", "Option 2"}
5. "searchable-dropdown" (Dropdown with multiple options inside a table with search). Eg. {{id = 1, name = "Option 1"}, {id = 2, name = "Option 2"}}

- Make sure to check the customs folder for example on how to create a custom panel and register their callbacks.
