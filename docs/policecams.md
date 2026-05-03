# Steps for installation

1. Drag and Drop the resource to your server. Make sure it starts at the end of the list so all the dependencies are loaded first.
2. Go through the config file and check all things to suit your server's need.
3. Images are present inside the assets folder. (Only required if you item based)

# Item Based

## For QBCore

- Add the following to your `qb-core/shared/items.lua` file:

```lua
["bodycam"] 			         = {["name"] = "bodycam", 		["label"] = "Bodycam", 		["weight"] = 2000, 		["type"] = "item", 		["image"] = "bodycam.png", ["unique"] = false, 	["useable"] = true, 	["shouldClose"] = true,	   	["combinable"] = nil,   ["description"] = "Bodycam"},
["dashcam"]                      = {["name"] = "dashcam", 		["label"] = "Dashcam", 		["weight"] = 2000, 		["type"] = "item", 		["image"] = "dashcam.png", ["unique"] = false, 	["useable"] = true, 	["shouldClose"] = true,	   	["combinable"] = nil,   ["description"] = "Dashcam"},
["policetablet"]                = {["name"] = "policetablet", 	["label"] = "Police Tablet", 	["weight"] = 2000, 		["type"] = "item", 		["image"] = "policetablet.png", ["unique"] = false, 	["useable"] = true, 	["shouldClose"] = true,	   	["combinable"] = nil,   ["description"] = "Police Tablet"},
["gpsmodule"]                   = {["name"] = "gpsmodule", 		["label"] = "GPS Module", 	["weight"] = 2000, 		["type"] = "item", 		["image"] = "gpsmodule.png", ["unique"] = false, 	["useable"] = true, 	["shouldClose"] = true,	   	["combinable"] = nil,   ["description"] = "GPS Module to setup Bait Cars"},
```

## For ESX

```sql
INSERT INTO `items` (`name`, `label`, `weight`, `rare`, `can_remove`) VALUES ('bodycam', 'Bodycam', 1, 0, 1) 
INSERT INTO `items` (`name`, `label`, `weight`, `rare`, `can_remove`) VALUES ('dashcam', 'Dashcam', 1, 0, 1)
INSERT INTO `items` (`name`, `label`, `weight`, `rare`, `can_remove`) VALUES ('policetablet', 'Police Tablet', 1, 0, 1)
INSERT INTO `items` (`name`, `label`, `weight`, `rare`, `can_remove`) VALUES ('gpsmodule', 'GPS Module', 1, 0, 1)
```

# Command Based

| Commands    | How it works |
| -------- | ------- |
| /bodycam  | Puts on/off bodycam    |
| /dashcam | Install Dashcam     |
| /baitcar    | Setup Baitcar    |
| /cams   | View all active cameras    |
| /removedashcam | Remove Dashcam    |
| /removebaitcar | Remove Baitcar    |
