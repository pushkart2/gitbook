# QB-PRINTER
A QBCore based FiveM script for making printers workable.

---
## Description
- Take printout of real documents using in-game prop of Photocopy/Printer machine. Add printer to any location by just adding the coords of location in **config.lua** file
```lua
Config.Printer = {
    [1] = {coords = vector3(-42.3, -1749.29, 29.42), heading = 320.32, SpawnModel = true, count = 50, capacity = 100},
    
    -- Add new coord and heading 
    -- SpawnModel (spawn machine prop or not at given coords)
    -- capacity (Maximum number of A4 sheets machine can carry)
    -- count (Initial count of A4 sheets in printer, when resource start)
}
```
## Dependencies
- ESX/QBCore
- ox_lib
- linden_inventory or ox-inventory or quasar inventory or qb inventory
- [interact-sound](https://github.com/qbcore-framework/interact-sound) (Optional if you want printing sounds. Check ogg file in assets and put that in interact-sound)

## Installation
- Drag and Drop assets file (given in assets folder) in mentioned folders

| File | Folder Path |
|------|-------------|
|printer.ogg|_interact-sound/client/html/sounds_|
|a4sheets.png|_linden_inventory/html/images_|
|printerdocument.png|_linden_inventory/html/images_|
# Linden Inventory

- Add items in **linden_inventory/shared/items.lua**, code snippet is given below
```lua
['a4sheets'] = {
    label = 'A4 Sheets',
    weight = 500,
    stack = true,
    close = false,
},

['printerdocument'] = {
    label = 'Printed Document',
    weight = 1000,
    stack = false,
    close = true,
    client = {}
},
```
- Update **linden_inventory/client/main.lua** file as mentioned in code snippet
- look for event - 'linden_inventory:useItem' and add an extra "and" like mentioned in the snippet

```lua
RegisterNetEvent('linden_inventory:useItem')
AddEventHandler('linden_inventory:useItem', function(item)
	if item ~= "printerdocument" and item.metadata.bag and not currentInventory then
		invOpen = false
		TriggerServerEvent('linden_inventory:openInventory', 'bag', { id = item.metadata.bag, label = item.label..' ('..item.metadata.bag..')', slot = item.slot, slots = item.metadata.slot or 5})
		return
	end
end)
```

- Set `Config.Inventory = "linden"` in config

# For OX Inventory

1. Set `Config.Inventory = "ox"` in config

2. Add the item in items.lua like this 

```lua
['a4sheets'] = {
		label = 'A4 Sheets',
		weight = 500,
		stack = true,
		close = false,
	},

	['printerdocument'] = {
		label = 'Printed Document',
		weight = 1000,
		stack = false,
		close = true,
		description = nil,
		client = {
			export = "snipe-printer.useDocument"
		},
	},
```

# For Quasar inventory

- Add the two items in your qs-core/config/config_items.lua 

```lua
    ["printerdocument"] = {
        ["name"] = "printerdocument",
        ["label"] = "Printer Document",                 
        ["weight"] = 50,         
        ["type"] = "item",         
        ["image"] = "printerdocument.png",     
        ["unique"] = true,         
        ["useable"] = true,     
        ["shouldClose"] = true,    
        ["combinable"] = nil,
        ["description"] = "Important Document"
    },
    ["a4sheets"] = {
        ["name"] = "a4sheets",
        ["label"] = "A4 Sheets",                 
        ["weight"] = 50,         
        ["type"] = "item",         
        ["image"] = "a4sheets.png",     
        ["unique"] = true,         
        ["useable"] = true,     
        ["shouldClose"] = true,    
        ["combinable"] = nil,
        ["description"] = "Blank Papers to insert into printer"
    },
```
- Set `Config.Inventory = "qs"` in config

# For QB Inventory

 Add items in **items.lua**, code snippet is given below
```lua
['printerdocument'] 			 = {['name'] = 'printerdocument', 				['label'] = 'Document', 				['weight'] = 500, 		['type'] = 'item', 		['image'] = 'printer_documents.png', 	['unique'] = true, 		['useable'] = true, 	['shouldClose'] = true,	   ['combinable'] = nil,   ['description'] = 'A nice document'},
["a4sheets"] 			 		 = {["name"] = "a4sheets", 						["label"] = "A4Sheets Pack", 			["weight"] = 2000, 		["type"] = "item", 		["image"] = "a4sheets.png", 		    ["unique"] = false, 	["useable"] = false, 	["shouldClose"] = true,	   ["combinable"] = nil,   ["description"] = "A bundle of 20 A4 Sheets"},
```
- Update **FormatItemInfo()** function in  _qb-inventory/html/js/**app.js**_ file as mentioned in code snippet
```javascript
function FormatItemInfo(itemData) {

    //Add else if condition for printerdocument item
    else if (itemData.name == "printerdocument") {
        $(".item-info-title").html('<p>'+itemData.label+'</p>')
        $(".item-info-description").html('<p>'+itemData.info.documentname+'</p><br/>');
    }

}
```

- Set `Config.Inventory = "qb"` in config

>Note: For any queries or help regarding installation, [Join our Discord](https://discord.com/invite/VGYkkAYVv2).
