---
icon: material/bed
---

# Motel

Rentable motel rooms with placeable furniture, stashes, and outfit storage. Supports two MLOs — **Opium Nights** and **Eclipse Tower**.

---

!!! warning "MLO compatibility"
    - Setup is identical for both MLOs.
    - You can only run **one MLO at a time**. Don't rename it. Delete the one you don't want.
    - Switching MLOs randomises furniture positions due to differing room sizes — clear your furniture before switching.
    - Stashes / outfits survive an MLO switch (players can re-place them).

The two MLOs differ in **room count** (Opium Nights: 405, Eclipse Tower: 276) and **room size**.

## :material-information-outline: How the system works

- The MLO has **405 rooms total**, so any server with ≤285 concurrent players can give every connected player their own room.
- Rooms are assigned on **player load** — the script finds an empty room and locks it to that player. When they leave, the room frees up for the next joiner.
- **Furniture follows the player.** When you're reassigned a different room, the furniture is automatically transferred and placed at equivalent offsets.
- Result: thousands of unique players supported without storage bloat — only currently-connected players occupy rooms.

## :material-cog: Config

- Read every option in `shared/open/config.lua`.
- Custom framework? Modify `client/open/cl_framework.lua` and `server/open/sv_framework.lua`.
- Inventory not in the supported list? Edit `client/open/cl_stash.lua`.
- Clothing script not in the supported list? Edit `client/open/cl_outfit.lua`.

## :material-database: SQL

Import the SQL file from the `SQL Install Me` folder.

## :material-tune: Optional — Nopixel Visuals fix

!!! note "Only applicable if you use Nopixel Visual mods"
    Find the file `ap1_04_bannerbld` and remove it — it draws an unwanted door texture on the entrance.

    Credits to **@mafiaborn**.

## :material-server: server.cfg

Add this line to `server.cfg`:

```
setr game_enableDynamicDoorCreation "true"
```

## :material-code-tags: Exports

### Client

```lua
exports["snipe-motel"]:isInRoom()              -- room number if inside a room, false otherwise
exports["snipe-motel"]:currentPlayerRoom()     -- player's owned room number, false if no room
exports["snipe-motel"]:OpenFurnitureMenu()     -- open the furniture menu (must be inside their owned room)
exports["snipe-motel"]:SpawnInsideApartment()  -- spawn inside the apartment (only if Config.RequireMoneyToRent = false)
exports["snipe-motel"]:getPlayerRoomLabel()    -- room label (used by the LilSeoul Motel MLO)
```

### Server

```lua
exports["snipe-motel"]:currentPlayerRoom(source) -- player's owned room number, false if no room
```

## :material-account-arrow-right: Optional — qb-spawn integration

!!! note "Existing characters only"
    First-time players still spawn through the standard QB spawn flow — only existing characters can spawn directly into the motel.

These changes are specifically for `qb-spawn`. For other spawn scripts, mirror the changes in your script.

### `config.lua`

Add the location:

```lua
["snipe_motel"] = { -- do not change the key
    coords   = vector3(-702.27, -2267.87, 13.46),
    location = "snipe_motel", -- do not change
    label    = "Opium Nights Motel",
},
```

### `client.lua`

Replace the `spawnplayer` NUI callback:

```lua
RegisterNUICallback('spawnplayer', function(data, cb)
    local location   = tostring(data.spawnloc)
    local type       = tostring(data.typeLoc)
    local ped        = PlayerPedId()
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
            local apartmentId   = insideMeta.apartment.apartmentId
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

## :material-help-circle: FAQ

??? question "Will this work with my custom MLO?"
    No. The script's zones are hardcoded for the shipped MLOs and would require huge work to port (furniture, elevators, doorlocks, etc). There is no path to swap MLOs.

??? question "Does this replace qb-apartments?"
    No. It's a standalone system and won't interfere with qb-apartments. Replacing qb-apartments is not on the roadmap due to multicharacter / spawn-selector edge cases.

??? question "Can I use just the MLO without the script?"
    No. The MLO without the script will likely crash the server because of the sheer volume of rooms/corridors loading.

??? question "Why does my room number change every restart?"
    Rooms are assigned on player load — that's how the system supports more unique players than there are physical rooms. Your furniture is synced and will always be placed at the equivalent offset in whichever room you're assigned.

??? question "Sometimes my furniture doesn't spawn"
    To prevent emote-based exploits into other players' rooms, furniture only spawns when the **owner** enters with the door **open**. Step out, leave the door open, walk back in, then close the door once the furniture appears.
