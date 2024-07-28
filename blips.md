# Installation

## Step 1 (Config)

- Configure the config properly based on the framework
- Check all the comments in the config file


## Step 2 (Items)

### Ox Inventory

- If you use ox_inventory, just follow this, do not add any other items below
```lua
["tracker"] = {
    label = "Tracker",
    weight = 500,
    stack = true,
    close = true,
    description = "Tracker that lets you show other officers",
    client = {
        image = "tracker.png",
        remove = function(total)
            pcall(function() return exports["snipe-blips"]:RemoveTrackerItem() end)
        end
    },
    
},

["gangtracker"] = {
    label = "gangtracker",
    weight = 500,
    stack = true,
    close = true,
    description = "Tracker that lets you show other Gang Members",
    client = {
        image = "tracker.png",
        remove = function(total)
            pcall(function() return exports["snipe-blips"]:RemoveTrackerItem() end)
        end
    },
    
},
```

- Add the items to your database/items.lua
### QBCore (Qb/lj inventory)
```lua
["tracker"] 			         = {["name"] = "tracker", 				        ["label"] = "Tracker", 		["weight"] = 500, 		["type"] = "item", 		["image"] = "tracker.png", ["unique"] = false, 	["useable"] = true, 	["shouldClose"] = true,	   	["combinable"] = nil,   ["description"] = "Tracker that lets you show other officers"},
["gangtracker"] 			         = {["name"] = "gangtracker", 				        ["label"] = "Gang Tracker", 		["weight"] = 500, 		["type"] = "item", 		["image"] = "tracker.png", ["unique"] = false, 	["useable"] = true, 	["shouldClose"] = true,	   	["combinable"] = nil,   ["description"] = "Tracker that lets you show other gang members"},
```



### ESX

```sql
INSERT INTO `items` (`name`, `label`, `weight`, `rare`, `can_remove`) VALUES ('tracker', 'Tracker', 1, 0, 1) 
INSERT INTO `items` (`name`, `label`, `weight`, `rare`, `can_remove`) VALUES ('gangtracker', 'Gang Tracker', 1, 0, 1) 
```


## Step 3 (Adding Export for item drop)

### QB/PS/LJ inventory



ps - https://github.com/Project-Sloth/ps-inventory/blob/84469dc164db586a5ceda24eca11be5ed94fc38a/server/main.lua#L228

qb - https://github.com/qbcore-framework/qb-inventory/blob/main/server/main.lua#L227

lj - https://github.com/loljoshie/lj-inventory/blob/main/server/main.lua#L209


Add this line at the lines mentioned
```lua
if item == "tracker" or item == "gangtracker" then -- if you have named the item something else, change it here
    TriggerClientEvent("snipe-blips:client:RemoveTrackerItem", source)
end
```

### ESX inventory (This is already configured. So no changes required)
