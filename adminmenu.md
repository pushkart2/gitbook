# Permissions
- In Config folder check `permissions.lua`, there are options to set if the particular role for god, admin or mod can access all the panels.

All the roles here can access the admin menu
Only the GOD can set the panels for the other roles
There are unlimited options
1. God -> can access all the commands
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

# How To add new UI Locales
- Please read each step properly. 
- To create a new locale, for eg. in language spanish, create a new file in html/locales called for eg. sp.json
- Copy the contents from en.json and put it in sp.json as is. Then modify it for your language. 
- Do not change the left side values, only the right side.
```json
"Revive": "Reanimar",
```
- Save the file and go to config.json in html folder and change lang to `sp` since your file is called `sp.json`
- Do not restart the script right away. Type `refresh` in server console first so it loads the newly created files and press enter and then do `ensure snipe-menu`.

# Spawning objects
- When spawning objects/job stashes, please look at the 2D text on the screen and use the right keys and everything. Each key is hardcoded and possibly wont be allowed to change.

# Keybinds
- Before starting the script on live server, go through cl_keybinds.lua file and see if you want to change the keybinds. Dont worry if you didnt configure, you can change them through settings. Go to GTA 5 Settings -> Keybinds -> Fivem and look for snipe-menu and change them.
- I would advise using mouse key for delete laser because it will be easier to access multiple keys.
- Keybinds only work if the dev mode is on. It can be turned on using the terminal button above the settings and only accessible to GodRoles.

# Discord Logging
- Change the webhook in sv_webhooks.lua. It logs almost each and every command used and who it was used on. There is not a lot of language options since it will be logged to a discord but you can change some things to your liking in config.

# Give Item
- There are some items which have metadata which needs to be taken care of while giving a item because if you dont, that item wont be useful at all.
- If you have a particular item which has metadata, you need to mention it in the sv_customise.lua -> GetItemMetadataInfo() function. I pass the item spawn name so you can use it to add if condition to add more items to your liking
# Miscellanuous
- There are bunch of client/server scripts which are unencrypted which you can edit it to your liking. 
- I wont be adding supports for other paid scripts but the logic required to edit will be made open if needed.

# Other
- I am open to suggestions about new panels. But dont expect it to get done right away.
- To add your suggestions, please create a thread in suggestions channel and post it there.
- By default, gang related buttons are disabled for ESX since ESX doesnt have default gangs.
- By default, open trunk and open glovebox have been disabled for ox inventory since there are some checks that prevent open trunk and glovebox without being near the car.
- Locales have been moved to locales.lua for ease.

# QBCore and ESX
## Permissions
- Check `permissions.lua` file under config folder.

## Clothing
```lua
Config.Clothing = "qb-clothing" -- qb-clothing || fivem-appearance || esx_skin || other
```
- If you choose other, please check `cl_customise_1.lua` and look for the event `snipe-menu:client:revertClothing` and add your custom event
- If you want to change event to give clothing menu, `check snipe-menu:server:giveClothes` event in sv_customise.lua

## Ox Inventory Trunk and Glovebox For Both QBCore and ESX

- For testing purpose until its stable, set `Config.Override` to true in config.lua and you will see the panels for open trunk and open glovebox (This is only for ox inventory). If you dont use ox_inventory please dont turn that to true.
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

# Ox Inventory Player Inventory For Both QBCore and ESX

## If you use the latest Ox inventory, you dont need to make the changes mentioned below. Just go to cl_customise and look for `snipe-menu:client:openinventory` event. Comment and uncomment the lines based on the comment there. 

# 

## Else follow the steps below
# 


- For opening player inventories, find the following lines in ox_inventory/server.lua under the following callback `lib.callback.register('ox_inventory:openInventory', function(source, inv, data)`

```lua
if right.coords == nil or #(right.coords - GetEntityCoords(GetPlayerPed(source))) < 10 then -- comment this line
	right.open = source
	left.open = right.id
else return end -- comment this line
```

- After commenting , the lines, it should look like
```lua
--if right.coords == nil or #(right.coords - GetEntityCoords(GetPlayerPed(source))) < 10 then -- comment this line
	right.open = source
	left.open = right.id
--else return end -- comment this line
```

![alt text](https://cdn.discordapp.com/attachments/704682484847345738/1054804324234051684/ox-inv-player.png)

- Another Change
- Add this following snippet at the top of the ox_inventory/client.lua
```lua
local isAdmin = false
function SetAdmin()
	isAdmin = true
end
exports('SetAdmin', SetAdmin)
```

- It should look like this

![alt text](https://cdn.discordapp.com/attachments/739152437645934632/1082684277742907473/ox_1.png)


- Inside the SetInterval, add the 3 lines as mentioned(Do not copy the logic as is, just copy the lines mentioned)
```lua
client.interval = SetInterval(function()
if invOpen == false then
	playerCoords = GetEntityCoords(playerPed)

	if currentWeapon and IsPedUsingActionMode(playerPed) then
		SetPedUsingActionMode(playerPed, false, -1, 'DEFAULT_ACTION')
	end
	isAdmin = false -- add this line
elseif invOpen == true then
	if not canOpenInventory() then
		client.closeInventory()
	else
		playerCoords = GetEntityCoords(playerPed)
		if currentInventory then

			if currentInventory.type == 'otherplayer' then
				local id = GetPlayerFromServerId(currentInventory.id)
				local ped = GetPlayerPed(id)
				local pedCoords = GetEntityCoords(ped)
				if not isAdmin then -- add this line
					if not id or #(playerCoords - pedCoords) > 1.8 or not (client.hasGroup(shared.police) or canOpenTarget(ped))  then
						client.closeInventory()
						lib.notify({ id = 'inventory_lost_access', type = 'error', description = locale('inventory_lost_access') })
					else
						TaskTurnPedToFaceCoord(playerPed, pedCoords.x, pedCoords.y, pedCoords.z, 50)
					end
				end -- add this line
			

			elseif currentInventory.coords and (#(playerCoords - currentInventory.coords) > (currentInventory.distance or 2.0) or canOpenTarget(playerPed)) then
				client.closeInventory()
				lib.notify({ id = 'inventory_lost_access', type = 'error', description = locale('inventory_lost_access') })
			end
		end
	end
end		
```	

- More Changes
- Looks for function `client.openInventory(inv, data)`
- add the two following lines mentioned in the snippet below inside that function

```lua
if not isAdmin then -- add this line
	if not targetCoords or #(targetCoords - GetEntityCoords(playerPed)) > 1.8 or not (client.hasGroup(shared.police) or canOpenTarget(targetPed))  then
		return lib.notify({ id = 'inventory_right_access', type = 'error', description = locale('inventory_right_access') })
	end
end -- add this line
```


# QBCore
## Opening stashes/trunk/glovebox (only for qb-inventory)

Add the two following events at the end of the inventory/client/main.lua (do not add it at the beginning please!!)

```lua
RegisterNetEvent('inventory:client:SetCurrentTrunk', function(vehicle)
    CurrentVehicle = vehicle
end)

RegisterNetEvent('inventory:client:SetCurrentGlovebox', function(vehicle)
    CurrentGlovebox = vehicle
end)
```

# ESX

## Config
```lua
Config.Core = "ESX" -- ESX or QBCore
Config.CoreFolderName = "es_extended"  -- es_extended || qb-core

Config.PlayerLoadedEvent = "esx:playerLoaded" -- esx:playerLoaded || QBCore:Client:OnPlayerLoaded
Config.PlayerUnloadEvent = "esx:onPlayerLogout " -- esx:onPlayerLogout || QBCore:Client:OnPlayerUnload           

Config.Target = "qtarget" -- qb-target || ox_target || qtarget (Only these 3 targets are supported. You will have to edit in cl_customise if you want to use any other target other than this. No support is given to other target scripts)
```

- If you have renamed events, you can rename them here. Make sure to choose the right target
## Inventory
```lua
Config.Inventory = "ox" -- qb or qs or ox 
```
- Only supported inventories are qb, ox or quasar. Most of the inventory logic is open in `sv_inventory.lua` and `cl_inventory.lua` if you want to support any other inventory.
## Bans
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

## Weather Sync
- cd easytime is configured by default for weather change. Logic is open in `cl_customise.lua` if you use a different weathersync script.

## Toggle Duty
- Toggle Duty event for ESX is open in `cl_customise_1.lua`. Look for event `snipe-menu:client:toggleDuty` and add your event near the comment.

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

# Files that are open to edit

-   `'config/config.lua',`
-   `'config/locales.lua',`
-   `'config/permissions.lua',`
-   `'client/cl_customise.lua',`
-   `'client/cl_keybinds.lua',`
-   `'client/cl_noclip.lua',`
-   `'client/cl_names.lua',`
-   `'server/sv_customise.lua',`
-   `'server/sv_webhooks.lua',`
-   `'server/sv_customise_new.lua',`
-   `'client/cl_vehdebug.lua',`
-   `'client/cl_customise_1.lua',`
-   `'client/pedlist.lua',`
-   `'server/sv_utils.lua',`
-   `'client/cl_utils.lua',`
-   `'server/sv_vehicles.lua',`
-   `'client/cl_inventory.lua',`
-   `'server/sv_inventory.lua',`