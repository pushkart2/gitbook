# Installation

- Configure the config properly based on the framework
- Images are present in assets folder

# Ox Inventory

## Item

```lua
	['beehive'] = { 
		label = 'Beehive',
		weight = 1,
		stack = false,
		consume = 0,
		server = {
			export = 'snipe-beekeeping.PlaceBeehive'
		},
	},

	['queen_bee'] = { 
		label = 'Queen Bee',
		weight = 1,
		stack = true,
		consume = 0,
	},

	['honey'] = { 
		label = 'Honey',
		weight = 1,
		stack = true,
		consume = 0,
	},

	['wax'] = { 
		label = 'Wax',
		weight = 1,
		stack = true,
		consume = 0,
	},

	['jelly'] = { 
		label = 'Jelly',
		weight = 1,
		stack = true,
		consume = 0,
	},
```

# QB Inventory

## Items

```lua
["beehive"] 			         = {["name"] = "beehive", 		["label"] = "Beehive", 		["weight"] = 2000, 		["type"] = "item", 		["image"] = "beehive.png", ["unique"] = false, 	["useable"] = true, 	["shouldClose"] = true,	   	["combinable"] = nil,   ["description"] = "Beehive"},
["queen_bee"] 			         = {["name"] = "queen_bee", 		["label"] = "Queen Bee", 		["weight"] = 2000, 		["type"] = "item", 		["image"] = "queen_bee.png", ["unique"] = false, 	["useable"] = true, 	["shouldClose"] = true,	   	["combinable"] = nil,   ["description"] = "Beehive"},
["honey"] 			         = {["name"] = "honey", 		["label"] = "Honey", 		["weight"] = 2000, 		["type"] = "item", 		["image"] = "honey.png", ["unique"] = false, 	["useable"] = true, 	["shouldClose"] = true,	   	["combinable"] = nil,   ["description"] = "Beehive"},
["wax"] 			         = {["name"] = "wax", 		["label"] = "Wax", 		["weight"] = 2000, 		["type"] = "item", 		["image"] = "wax.png", ["unique"] = false, 	["useable"] = true, 	["shouldClose"] = true,	   	["combinable"] = nil,   ["description"] = "Beehive"},
["jelly"] 			         = {["name"] = "jelly", 		["label"] = "Jelly", 		["weight"] = 2000, 		["type"] = "item", 		["image"] = "jelly.png", ["unique"] = false, 	["useable"] = true, 	["shouldClose"] = true,	   	["combinable"] = nil,   ["description"] = "Beehive"},
```