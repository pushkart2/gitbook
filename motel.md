# Config

- There are bunch of options in shared/open/config.lua. Make sure to go through each of them along with comments to understand what they do.
- If you use custom framework, you will have to modify client/open/cl_framework.lua and server/open/sv_framework.lua to make it work with your framework.
- If you use any inventory other than the ones mentioned, you will have to edit client/open/cl_stash.lua
- If you use any other clothing script other than the ones mentioned, you will have to edit client/open/cl_outfit.lua

# SQL
- Import the sql file present in SQL Install Me folder to your database.

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

```

Server Exports:

```lua
exports["snipe-motel"]:currentPlayerRoom(source) -- returns the players current room number and false if he doesnt own a room
```

# FAQ

### Does this script with my custom MLOS?

Answer: No, this script will only work with the MLO provided with the script. The reason being, the zones are hardcoded for the MLO and requires a lot of work for furniture, elevators, doorlocks, etc. This script will not work with any other MLO. There is no way to edit the zones/doorlock etc to make it work with other MLOS. 

### Will this script replace qb-apartments?

Answer: No, this script will not replace qb-apartments. This script is a standalone script and will not interfere with any other scripts. No support or changes will be done to make it replace qb-apartments due to issues with multiple multicharacters/spawn selectors/ etc. 

### Can I only use the MLO?

Answer: No, the MLO cannot be used standalone. I might plan on releasing just the MLO in future but for now, if you use the MLO without the script, it will probably crash the server due to the amount of rooms/corridors loading.

### Why do my room number changes every restart?
Answer: The room number is assigned on player load. This is to ensure so say your server max player count is 250 but you have 1000 unique players, every player gets a room on load. Your furniture is synced and if you place it in 1 room, it will be placed at the same place in another room that is assigned to you.

### Sometimes my furniture wont spawn, why?
Answer: To avoid people exploiting and going to other people's room using emotes or whatever, the furniture is only spawned when player enters the room and the door is open for that room. So if you are owner of the room and the door is closed, the furniture will not spawn. So make sure to step out of the room, keep the door open, then enter the room and close the door after you see the furniture. 