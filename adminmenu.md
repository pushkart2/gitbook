# Step 1 (Config)

## ESX
- Configure the config

```lua
Config.Core = "ESX" -- ESX or QBCore
Config.CoreFolderName = "es_extended"  -- es_extended || qb-core

Config.PlayerLoadedEvent = "esx:playerLoaded" -- esx:playerLoaded || QBCore:Client:OnPlayerLoaded
Config.PlayerUnloadEvent = "esx:onPlayerLogout " -- esx:onPlayerLogout || QBCore:Client:OnPlayerUnload           

Config.Target = "qtarget" -- qb-target || ox_target || qtarget (Only these 3 targets are supported. You will have to edit in cl_customise if you want to use any other target other than this. No support is given to other target scripts)
```

## QBCore
- Config is by default set to QBCore. 

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

## Ox Inventory

- For trunk and glovebox, make the following changes in ox_inventory/server.lua, look for the following the function `local function openInventory(source, invType, data, ignoreSecurityChecks)`

look for the following code block. Do not copy this as is, please copy the lines mentioned as `add this line` and add them one by one
```lua
elseif type(data) == 'table' then
if data.netid then
	data.type = invType
	right = Inventory(data)
elseif invType == 'drop' then
	right = Inventory(data.id)
elseif invType == 'trunk' then -- add this line
	right = Inventory(data.id) -- add this line
elseif invType == 'glovebox' then -- add this line
	right = Inventory(data.id) -- add this line
else
	return
```
![alt text](https://cdn.discordapp.com/attachments/704682484847345738/1054804324540239892/ox-inv-trunk-glove.png)

## Changes in ox_inventory/modules/inventory/server.lua

- Look for function `loadInventoryData()` around line 54.
- 3 lines in that function needs to be changed

```lua
if not inventory then
local entity

if data.netid then
	entity = NetworkGetEntityFromNetworkId(data.netid)

	if not entity and not exports["snipe-menu"]:isAdmin(source) then -- line changed
		return shared.info('Failed to load vehicle inventory data (no entity exists with given netid).')
	end
else
	local vehicles = GetAllVehicles()

	for i = 1, #vehicles do
		local vehicle = vehicles[i]
		local _plate = GetVehicleNumberPlateText(vehicle)

		if _plate:find(plate) then
			entity = vehicle
			data.netid = NetworkGetNetworkIdFromEntity(entity)
			break
		end
	end

	if not entity and not exports["snipe-menu"]:isAdmin(source) then -- line changed
		return shared.info('Failed to load vehicle inventory data (no entity exists with given plate).')
	end
end

if not source then
	source = NetworkGetEntityOwner(entity)

	if not source and not exports["snipe-menu"]:isAdmin(source) then -- line changed
		return shared.info('Failed to load vehicle inventory data (entity is unowned).')
	end
end
```

## Changes in ox_inventory/client.lua

Around line 271, look for this part of code `if inv == 'trunk' then` and replace it with 
`if inv == 'trunk' and not exports["snipe-menu"]:isAdmin() then`

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

# Illenium And ESX

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

Replace the following event in snipe-menu/server/open/sv_clothing.lua
```lua
RegisterServerEvent('snipe-menu:server:giveClothes', function(otherPlayerId)
    local src = source
    if src ~= 0 and onlineAdmins[src] then
        SendLogs(src, "triggered", Config.Locales["give_clothes_used"]..GetPlayerName(otherPlayerId))
        if Config.Core == "QBCore" then
            TriggerClientEvent('qb-clothing:client:openMenu', otherPlayerId)
        elseif Config.Core == "ESX" then
            TriggerClientEvent("snipe-menu:client:openAppearance", otherPlayerId)
        end
    else
        TriggerServerEvent("snipe-menu:server:sendLogs", "exploit", Config.Locales["clothes_exploit_event"])
    end
end)
```

# Step 8 (Optional)
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

# Step 8 (Optional Exports)
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
