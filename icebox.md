# snipe-icebox

# Inventory Changes (Only for QBCore Framework)

## qb-inventory / ps-inventory/ lj-inventory

- look for this function `local function RemoveItem(source, item, amount, slot)` in inventory/server/main.lua
- add this code below the following line
```lua
if not Player then return false end
TriggerClientEvent("snipe-icebox:client:removeItem", source, item) -- add this line
```

# Items
- If you have any of the item from other scripts, dont add it again. 
## qb-inventory

```lua
	["prop_chain"]						 = {["name"] = "prop_chain", 						["label"] = "Chain as Prop", 			["weight"] = 0, 		["type"] = "item", 		["image"] = "prop_chain.png", 					["unique"] = true, 	["useable"] = true, 	["shouldClose"] = true,	   		["combinable"] = nil,   ["description"] = "Example Chain for Prop"},
	["clothing_chain"]						 = {["name"] = "clothing_chain", 						["label"] = "Chain as Clothing", 	["weight"] = 0, 		["type"] = "item", 		["image"] = "clothing_chain.png", 					["unique"] = false, 	["useable"] = true, 	["shouldClose"] = true,	   		["combinable"] = nil,   ["description"] = "Example Chain for Clothing item"},
    ["clothing_watch"]						 = {["name"] = "clothing_watch", 						["label"] = "Watch as Clothing", 	["weight"] = 0, 		["type"] = "item", 		["image"] = "clothing_watch.png", 					["unique"] = false, 	["useable"] = true, 	["shouldClose"] = true,	   		["combinable"] = nil,   ["description"] = "Example Watch for Clothing item"},
	["ruby"]						 = {["name"] = "ruby", 						["label"] = "Ruby", 				["weight"] = 0, 		["type"] = "item", 		["image"] = "ruby.png", 				["unique"] = false, 	["useable"] = false, 	["shouldClose"] = true,	   		["combinable"] = nil,   ["description"] = "Ruby"},
    ["buff_diamond"]						 = {["name"] = "buff_diamond", 						["label"] = "Diamond(Buff)", 				["weight"] = 0, 		["type"] = "item", 		["image"] = "diamond.png", 				["unique"] = false, 	["useable"] = false, 	["shouldClose"] = true,	   		["combinable"] = nil,   ["description"] = "Diamonds use to buff chains/watches"},
    ["buff_ruby"]						 = {["name"] = "buff_ruby", 						["label"] = "Ruby(Buff)", 				["weight"] = 0, 		["type"] = "item", 		["image"] = "ruby.png", 				["unique"] = false, 	["useable"] = false, 	["shouldClose"] = true,	   		["combinable"] = nil,   ["description"] = "Ruby use to buff chains/watches"},
    ['real_chain_tester'] = { ['name'] = 'real_chain_tester', ['label'] = 'Chain Tester (Real)', ["description"] = "Chain Tester which will let you test chain for other people", ['weight'] = 500, ['type'] = 'item', ['image'] = 'tester.png', ['unique'] = true, ['useable'] = true, ['shouldClose'] = false, ['combinable'] = nil, ['description'] = 'none' },
    ['fake_chain_tester'] = { ['name'] = 'fake_chain_tester', ['label'] = 'Chain Tester (Fake)', ["description"] = "Chain Tester which will let you test chain for other people", ['weight'] = 500, ['type'] = 'item', ['image'] = 'tester.png', ['unique'] = true, ['useable'] = true, ['shouldClose'] = false, ['combinable'] = nil, ['description'] = 'none' },
```

## qs-inventory

```lua
    ['prop_chain'] = {
        ['name'] = 'prop_chain',
        ['label'] = 'Chain as Prop',
        ['weight'] = 500,
        ['type'] = 'item',
        ['image'] = 'prop_chain.png',
        ['unique'] = true,
        ['useable'] = true,
        ['shouldClose'] = false,
        ['combinable'] = nil,
        ['description'] = 'none'
    },

    ['clothing_chain'] = {
        ['name'] = 'clothing_chain',
        ['label'] = 'Chain as Clothing',
        ['weight'] = 500,
        ['type'] = 'item',
        ['image'] = 'clothing_chain.png',
        ['unique'] = true,
        ['useable'] = true,
        ['shouldClose'] = false,
        ['combinable'] = nil,
        ['description'] = 'none'
    },
    ['clothing_watch'] = {
        ['name'] = 'clothing_watch',
        ['label'] = 'Watch as Clothing',
        ['weight'] = 500,
        ['type'] = 'item',
        ['image'] = 'clothing_watch.png',
        ['unique'] = true,
        ['useable'] = true,
        ['shouldClose'] = false,
        ['combinable'] = nil,
        ['description'] = 'none'
    },

    ['diamond'] = {
        ['name'] = 'diamond',
        ['label'] = 'Diamond',
        ['weight'] = 500,
        ['type'] = 'item',
        ['image'] = 'diamond.png',
        ['unique'] = true,
        ['useable'] = true,
        ['shouldClose'] = false,
        ['combinable'] = nil,
        ['description'] = 'none'
    },

    ['ruby'] = {
        ['name'] = 'ruby',
        ['label'] = 'Ruby',
        ['weight'] = 500,
        ['type'] = 'item',
        ['image'] = 'ruby.png',
        ['unique'] = true,
        ['useable'] = true,
        ['shouldClose'] = false,
        ['combinable'] = nil,
        ['description'] = 'none'
    },

    ['buff_diamond'] = {
        ['name'] = 'buff_diamond',
        ['label'] = 'Diamond(Buff)',
        ['weight'] = 500,
        ['type'] = 'item',
        ['image'] = 'diamond.png',
        ['unique'] = true,
        ['useable'] = true,
        ['shouldClose'] = false,
        ['combinable'] = nil,
        ['description'] = 'none'
    },

    ['buff_ruby'] = {
        ['name'] = 'buff_ruby',
        ['label'] = 'Ruby(Buff)',
        ['weight'] = 500,
        ['type'] = 'item',
        ['image'] = 'ruby.png',
        ['unique'] = true,
        ['useable'] = true,
        ['shouldClose'] = false,
        ['combinable'] = nil,
        ['description'] = 'none'
    },

    ['real_chain_tester'] = {
        ['name'] = 'real_chain_tester',
        ['label'] = 'Chain Tester (Real)',
        ['weight'] = 500,
        ['type'] = 'item',
        ['image'] = 'tester.png',
        ['unique'] = true,
        ['useable'] = true,
        ['shouldClose'] = false,
        ['combinable'] = nil,
        ['description'] = 'none'
    },
    ['fake_chain_tester'] = {
        ['name'] = 'fake_chain_tester',
        ['label'] = 'Chain Tester (Fake)',
        ['weight'] = 500,
        ['type'] = 'item',
        ['image'] = 'tester.png',
        ['unique'] = true,
        ['useable'] = true,
        ['shouldClose'] = false,
        ['combinable'] = nil,
        ['description'] = 'none'
    },
	```

## ox inventory
- if you add more chains, make sure to follow the export and everything properly so it triggers the export on use of the chain
```lua
    ["clothing_chain"] = {
        label = "Clothing Chain",
        weight = 1,
        stack = false,
        close = true,
        consume = 0,
        server = {
            export = 'snipe-icebox.useChain',
        }
    },
    ["prop_chain"] = {
        label = "Prop Chain",
        weight = 1,
        stack = false,
        close = true,
        consume = 0,
        server = {
            export = 'snipe-icebox.useChain',
        }
    },

    ["clothing_watch"] = {
        label = "Clothing Watch",
        weight = 1,
        stack = false,
        close = true,
        consume = 0,
        server = {
            export = 'snipe-icebox.useWatch',
        }
    },

    ["diamond"] = {
        label = "Diamond",
        weight = 1,
        stack = true,
        close = true,
    },

    ["ruby"] = {
        label = "Ruby",
        weight = 1,
        stack = true,
        close = true,
    },

    ["buff_diamond"] = {
		label = "Buff Diamond",
		weight = 1,
		stack = true,
		close = true,
	},
    
    ["buff_ruby"] = {
		label = "Buff Ruby",
		weight = 1,
		stack = true,
		close = true,
	},

    ["real_chain_tester"] = {
        label = "Chain Tester (Real)",
        weight = 1,
        stack = true,
        close = true,
    },
    ["fake_chain_tester"] = {
        label = "Chain Tester (Fake)",
        weight = 1,
        stack = true,
        close = true,
    },
    ```

# Optional changes

- If you use chains as props, its advisable to remove chain when a player enters or leaves an apartment or a house. 
- Whenever player enter the house, just add this exports `exports["snipe-icebox"]:RemoveChains()` and the same export when they leave the house so there is no tornado effect when they sit in the car. 

# Known Issues

- If you are unable to see the ui at position but if you press E, it opens the context menu or safe, just set the `Config.UseLibTextUI` to false. Read the comments on that line
- The particle effects are not showing. You probably didnt set the offset properly in Config.Chains for that effect or that effect doesnt work. Always test the base effects given first to see if the effects are visible with new infuse items.