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

- For trunk and glovebox, make the following changes in ox_inventory/server.lua, look for the following the callback `lib.callback.register('ox_inventory:openInventory', function(source, inv, data)`

look for the following code block. Do not copy this as is, please copy the lines mentioned as `add this line` and add them one by one
```lua
elseif type(data) == 'table' then
if data.netid then
	data.type = inv
	right = Inventory(data)
elseif inv == 'drop' then
	right = Inventory(data.id)
elseif inv == 'trunk' then -- add this line
	right = Inventory(data.id) -- add this line
elseif inv == 'glovebox' then -- add this line
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

# Step 5 (Bans (Only for ESX))

- Run the `bans.sql` file given in sql folder
- Make the following changes in es_extended/server/main.lua
```lua
AddEventHandler('playerConnecting', function(name, setCallback, deferrals)
	deferrals.defer()
	local playerId = source
	local identifier = ESX.GetIdentifier(playerId)
	Wait(100)
	local isBanned, Reason = exports["snipe-menu"]:IsPlayerBanned(playerId) -- added
	if identifier then
		if ESX.GetPlayerFromIdentifier(identifier) then
			deferrals.done(('There was an error loading your character!\nError code: identifier-active\n\nThis error is caused by a player on this server who has the same identifier as you have. Make sure you are not playing on the same account.\n\nYour identifier: %s'):format(identifier))
		else
			if isBanned then -- added
				deferrals.done(Reason) -- added
			else -- added
				deferrals.done()
			end -- added
		end
	else
		deferrals.done('There was an error loading your character!\nError code: identifier-missing\n\nThe cause of this error is not known, your identifier could not be found. Please come back later or report this problem to the server administration team.')
	end
end)
```

If you use multicharacter script (esx-multicharacter), make the following changes in `esx-multicharacter/server/main.lua`
```lua
AddEventHandler('playerConnecting', function(playerName, setKickReason, deferrals)
    deferrals.defer()
    local playerId = source
    local identifier = GetIdentifier(source)

    Wait(100)
    local isBanned, Reason = exports["snipe-menu"]:IsPlayerBanned(playerId) -- added
    if identifier then
        if ESX.Players[identifier] then
            deferrals.done(('A player is already connected to the server with this identifier.\nYour identifier: %s:%s'):format(PRIMARY_IDENTIFIER, identifier))
        else
            if isBanned then -- added
                deferrals.done(Reason) -- added
            else -- added
                deferrals.done()
            end -- added
        --    deferrals.done()
        end
    else
        deferrals.done(('Unable to retrieve player identifier.\nIdentifier type: %s'):format(PRIMARY_IDENTIFIER))
    end
end)
```

# Step 6 (Optional)
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

# Step 7 (Optional Exports)
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
