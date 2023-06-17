# Installation

- Configure the config properly based on the framework
- Images are present in assets folder
# Requirements

- [SliderGame](https://github.com/pushkart2/slidergame)
- Target (qb, q or ox)

# For QB Inventory

## Items
```lua
["methtable"]= {["name"] = "methtable", ["label"] = "Meth Table", 		["weight"] = 2000, 		["type"] = "item", 		["image"] = "methtable.png", ["unique"] = true, 	["useable"] = true, 	["shouldClose"] = true,	   	["combinable"] = nil,   ["description"] = "A Table"},
["methbag"]= {["name"] = "methbag", ["label"] = "Meth Bag", 		["weight"] = 1000, 		["type"] = "item", 		["image"] = "meth10g.png", ["unique"] = true, 	["useable"] = true, 	["shouldClose"] = true,	   	["combinable"] = nil,   ["description"] = "Meth Bag"},
```
## Inventory changes

1. inventory/html/js/app.js

Look for  ```function FormatItemInfo```

Add this two else if statements below labkey (labkey part already exists)

```js
else if (itemData.name == "labkey") {
    $(".item-info-title").html("<p>" + itemData.label + "</p>");
    $(".item-info-description").html("<p>Lab: " + itemData.info.lab + "</p>");
} 
else if (itemData.name == "methtable") {
    $(".item-info-title").html("<p>" + itemData.label + "</p>");
    $(".item-info-description").html("<p>Table ID: " + itemData.info.tableid + "</p>");
}
else if (itemData.name == "methbag") {
    $(".item-info-title").html("<p>" + itemData.label + "</p>");
    $(".item-info-description").html("<p>Quality: " + itemData.info.qualityIndex + "</p>");
}
```

2. inventory/server/main.lua (this is optional change if you spawn table and methbag)

Look for ```QBCore.Commands.Add("giveitem"```

add this two else if statements (labkey part already exists)

```lua
elseif itemData["name"] == "labkey" then
    info.lab = exports["qb-methlab"]:GenerateRandomLab()
elseif itemData["name"] == "methtable" then
    info.tableid = tostring(QBCore.Shared.RandomInt(5) .. QBCore.Shared.RandomStr(2) .. QBCore.Shared.RandomInt(6))
elseif itemData["name"] == "methbag" then
    info.quality = math.random(1,100)					
```

# For OX Inventory

## Items
```lua
['methtable'] = {
    label = 'Meth Table',
    weight = 220,
    stack = false,
    consume = 0,
    client = {
        export = 'snipe-meth.UseTable'
    }
},
['methbag'] = {
    label = 'Meth Bag',
    weight = 220,
    stack = false,
    consume = 0,
    client = {
        export = 'snipe-meth.UseBag'
    }
},
['crack_baggy'] = {
    label = 'Bag of Crack',
    weight = 220,
    stack = true,
    consume = 0,
},
```

# For Quasar inventory

## Items
```lua
["methtable"]= {
    ["name"] = "methtable", 
    ["label"] = "Meth Table", 		
    ["weight"] = 2000, 		
    ["type"] = "item", 		
    ["image"] = "methtable.png", 
    ["unique"] = true, 	
    ["useable"] = true, 	
    ["shouldClose"] = true,	   	
    ["combinable"] = nil,   
    ["description"] = "A Table"
},
["methbag"]= {
    ["name"] = "methbag", 
    ["label"] = "Meth Bag", 		
    ["weight"] = 1000, 		
    ["type"] = "item", 		
    ["image"] = "meth10g.png", 
    ["unique"] = true, 	
    ["useable"] = true, 	
    ["shouldClose"] = true,	   	
    ["combinable"] = nil,   
    ["description"] = "Meth Bag"
},
["crack_baggy"]= {
    ["name"] = "crack_baggy", 
    ["label"] = "Bag of Crack", 		
    ["weight"] = 100, 		
    ["type"] = "item", 		
    ["image"] = "meth10g.png", 
    ["unique"] = false, 	
    ["useable"] = true, 	
    ["shouldClose"] = true,	   	
    ["combinable"] = nil,   
    ["description"] = "Small bag of crack"
},
```