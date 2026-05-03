# IMPORTANT

- Installation for both MLOs (Opium Nights or Eclipse Tower) is exactly the same. 
- If you bought both the MLO's, you can only use 1 at a time and make sure the mlo is not renamed. Delete the MLO you dont want to use.
- If you switch from one MLO to another, the furnitures in your room will be at random location due to the size of the rooms and the offsets. If you want to switch, delete all the furnitures. 
- Stashes/Outfits can stay because the player can reset them again.

- Difference between the two is the room numbers (405 in Opium Nights and 276 in Eclipse Tower) and the size of the rooms.

# How the Motel System Works

There are total of 405 rooms in the MLO, which means if you have a server population of 285 or lower, every player in your server will be assigned a room. 
The room number is assigned on Player Load. So the system looks for an empty room and assigns it to the player. Once the player leaves, that room is vacant and is assigned to someone else who joins.

So what about the furniture?
- Furniture placed in your room is transferred to another room that is assigned to you. The offsets are calculated and the furniture is placed at the same place.

This whole feature lets you handle 1000's of unique players without any headache. 
The system only cares about people in server and not the other people.

# Config

- There are bunch of options in shared/open/config.lua. Make sure to go through each of them along with comments to understand what they do.
- If you use custom framework, you will have to modify client/open/cl_framework.lua and server/open/sv_framework.lua to make it work with your framework.
- If you use any inventory other than the ones mentioned, you will have to edit client/open/cl_stash.lua
- If you use any other clothing script other than the ones mentioned, you will have to edit client/open/cl_outfit.lua

# SQL
- Import the sql file present in SQL Install Me folder to your database.

# Nopixel Visuals (Optional)
- People using Motel System with Nopixel Visual mods, look for the following file `ap1_04_bannerbld` and remove it. It creates a door texture on the entrance.

- Credits @mafiaborn 

# Server.CFG changes
Add the following line to server.cfg

```
setr game_enableDynamicDoorCreation "true"
```

# Exports

Client Exports:

```lua
exports["snipe-motel"]:isInRoom() -- returns room number if in room, false if not

exports["snipe-motel"]:currentPlayerRoom() -- returns players current room number and false if he doesnt own a room

exports["snipe-motel"]:OpenFurnitureMenu() -- Opens the furnitures menu. Only opens when the player is inside the room and he owns the room

exports["snipe-motel"]:SpawnInsideApartment() -- Spawns the player inside the apartment. Only works if the Config.RequireMoneyToRent is set to false

exports["snipe-motel"]:getPlayerRoomLabel() -- returns players room label (used for LilSeoul Motel MLO)
```

Server Exports:

```lua
exports["snipe-motel"]:currentPlayerRoom(source) -- returns the players current room number and false if he doesnt own a room
```

# QB Spawn Changes (Optional)
- If you want a player to spawn inside apartment, you will have to do the following changes.
- NOTE: This wont spawn the new player inside the motel. Only after the first time, they will be able to spawn inside the motel
- These are the changes only if you use qb-spawn. If you use any other spawn script, you will have to modify that script. Just see the changes I did and make similar changes to your script.

## config.lua changes
- add the following location.
```lua
["snipe_motel"] = { -- do not change
    coords = vector3(-702.27, -2267.87, 13.46), 
    location = "snipe_motel", -- do not change
    label = "Opium Nights Motel",
},
```

## client.lua changes
- replace the following nui callback
```lua
RegisterNUICallback('spawnplayer', function(data, cb)
    local location = tostring(data.spawnloc)
    local type = tostring(data.typeLoc)
    local ped = PlayerPedId()
    local PlayerData = QBCore.Functions.GetPlayerData()
    local insideMeta = PlayerData.metadata["inside"]
    if type == "current" then
        PreSpawnPlayer()
        QBCore.Functions.GetPlayerData(function(pd)
            ped = PlayerPedId()
            SetEntityCoords(ped, pd.position.x, pd.position.y, pd.position.z)
            SetEntityHeading(ped, pd.position.a)
            FreezeEntityPosition(ped, false)
        end)

        if insideMeta.house ~= nil then
            local houseId = insideMeta.house
            TriggerEvent('qb-houses:client:LastLocationHouse', houseId)
        elseif insideMeta.apartment.apartmentType ~= nil or insideMeta.apartment.apartmentId ~= nil then
            local apartmentType = insideMeta.apartment.apartmentType
            local apartmentId = insideMeta.apartment.apartmentId
            TriggerEvent('qb-apartments:client:LastLocationHouse', apartmentType, apartmentId)
        end
        TriggerServerEvent('QBCore:Server:OnPlayerLoaded')
        TriggerEvent('QBCore:Client:OnPlayerLoaded')
        PostSpawnPlayer()
    elseif type == "house" then
        PreSpawnPlayer()
        TriggerEvent('qb-houses:client:enterOwnedHouse', location)
        TriggerServerEvent('QBCore:Server:OnPlayerLoaded')
        TriggerEvent('QBCore:Client:OnPlayerLoaded')
        TriggerServerEvent('qb-houses:server:SetInsideMeta', 0, false)
        TriggerServerEvent('qb-apartments:server:SetInsideMeta', 0, 0, false)
        PostSpawnPlayer()
    elseif type == "normal" then
        PreSpawnPlayer()
        TriggerServerEvent('QBCore:Server:OnPlayerLoaded')
        TriggerEvent('QBCore:Client:OnPlayerLoaded')
        TriggerServerEvent('qb-houses:server:SetInsideMeta', 0, false)
        TriggerServerEvent('qb-apartments:server:SetInsideMeta', 0, 0, false)
        if location == "snipe_motel" then
            Wait(2000)
            exports["snipe-motel"]:SpawnInsideApartment()
            PostSpawnPlayer()
        else
            local pos = QB.Spawns[location].coords
            SetEntityCoords(ped, pos.x, pos.y, pos.z)
            Wait(500)
            SetEntityCoords(ped, pos.x, pos.y, pos.z)
            SetEntityHeading(ped, pos.w)
            PostSpawnPlayer()
        end
    end
    cb('ok')
end)

```

# FAQ

### Will this script work with my custom MLOS?

Answer: No, this script will only work with the MLO provided with the script. The reason being, the zones are hardcoded for the MLO and requires a lot of work for furniture, elevators, doorlocks, etc. This script will not work with any other MLO. There is no way to edit the zones/doorlock etc to make it work with other MLOS. 

### Will this script replace qb-apartments?

Answer: No, this script will not replace qb-apartments. This script is a standalone script and will not interfere with any other scripts. No support or changes will be done to make it replace qb-apartments due to issues with multiple multicharacters/spawn selectors/ etc. 

### Can I only use the MLO?

Answer: No, the MLO cannot be used standalone. I might plan on releasing just the MLO in future but for now, if you use the MLO without the script, it will probably crash the server due to the amount of rooms/corridors loading.

### Why do my room number changes every restart?
Answer: The room number is assigned on player load. This is to ensure so say your server max player count is 250 but you have 1000 unique players, every player gets a room on load. Your furniture is synced and if you place it in 1 room, it will be placed at the same place in another room that is assigned to you.

### Sometimes my furniture wont spawn, why?
Answer: To avoid people exploiting and going to other people's room using emotes or whatever, the furniture is only spawned when player enters the room and the door is open for that room. So if you are owner of the room and the door is closed, the furniture will not spawn. So make sure to step out of the room, keep the door open, then enter the room and close the door after you see the furniture. 