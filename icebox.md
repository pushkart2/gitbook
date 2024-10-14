# Icebox Script
- Buy here - https://snipe.tebex.io/package/5781971
- Support - https://discord.gg/AeCVP2F8h7

# READ IMPORTANT
- People who owned the icebox before the update (Oct 10 2024), a lot of new parameters are added to each chain/watch. The new parameters are `itemPrice` and `shopType`. 
- `itemPrice` is the price of the item if you enable a buy location. If you dont specify the price for the chain/watch, it will take the default price from the config.lua
- `shopType` is the shop the where the chain/watch will be available. If you dont specify the shopType, it will be available in all the shops. 
- Based on the new two values, you can set some items to be available in 1 shop and not in other. With this you can for eg create a new shop called as mask shop and just add the mask items to this shop. 
- Adding the item price and setting up buy locations will take away the headache of crafting the chains/watches and people can directly buy it at a certain price. 
- Also now people can wear multiple chains and watches at the same time as long as they belong to different componentId and propId. So if you have a chain under componentId 7 and other chain under componentId 9, you can wear both of them at the same time.
- You can still use your old chains.lua and watches.lua without breaking any functionality. But if you want to use these new features, you need to add these parameters manually to every chain/watch. 


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
    ["mask_pigface"]						 = {["name"] = "mask_pigface", 						["label"] = "Pigface Mask", 	["weight"] = 0, 		["type"] = "item", 		["image"] = "mask_pigface.png", 					["unique"] = false, 	["useable"] = true, 	["shouldClose"] = true,	   		["combinable"] = nil,   ["description"] = "Example Chain for Mask Item"},
    ["clothing_watch"]						 = {["name"] = "clothing_watch", 						["label"] = "Watch as Clothing", 	["weight"] = 0, 		["type"] = "item", 		["image"] = "clothing_watch.png", 					["unique"] = false, 	["useable"] = true, 	["shouldClose"] = true,	   		["combinable"] = nil,   ["description"] = "Example Watch for Clothing item"},
	["ruby"]						 = {["name"] = "ruby", 						["label"] = "Ruby", 				["weight"] = 0, 		["type"] = "item", 		["image"] = "ruby.png", 				["unique"] = false, 	["useable"] = false, 	["shouldClose"] = true,	   		["combinable"] = nil,   ["description"] = "Ruby"},
    ["buff_diamond"]						 = {["name"] = "buff_diamond", 						["label"] = "Diamond(Buff)", 				["weight"] = 0, 		["type"] = "item", 		["image"] = "diamond.png", 				["unique"] = false, 	["useable"] = false, 	["shouldClose"] = true,	   		["combinable"] = nil,   ["description"] = "Diamonds use to buff chains/watches"},
    ["buff_ruby"]						 = {["name"] = "buff_ruby", 						["label"] = "Ruby(Buff)", 				["weight"] = 0, 		["type"] = "item", 		["image"] = "ruby.png", 				["unique"] = false, 	["useable"] = false, 	["shouldClose"] = true,	   		["combinable"] = nil,   ["description"] = "Ruby use to buff chains/watches"},
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
    ['mask_pigface'] = {
        ['name'] = 'mask_pigface',
        ['label'] = 'PigFace Mask',
        ['weight'] = 500,
        ['type'] = 'item',
        ['image'] = 'mask_pigface.png',
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
    ["mask_pigface"] = {
        label = "PigFace Mask",
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
```

# How to install new items

- Create items in your inventory based on the clothing_chain or clothing_watch mentioned in the installation step for your inventory.
- If you use qb/qs inventory, add the items in your inventory the way you add other items.

Eg. for ox_inventory chain (THIS IS FOR OX_INVENTORY!! DO NOT COPY PASTE THIS FOR QB/QS INVENTORY)
```lua
["test_chain"] = {
    label = "Test Chain",
    weight = 1,
    stack = false,
    close = true,
    consume = 0,
    server = {
        export = 'snipe-icebox.useChain',
    }
},
```
- Configure the chain/watch in `snipe-icebox/shared/chains.lua` or `snipe-icebox/shared/watches.lua`

Eg.
```lua
["test_chain"] = { -- item name
    label = "Test Chain", -- item label that will show on the context menu
    drawable = true, -- if the chain is clothing item
    category = "business", -- category of the chain
    itemPrice = 5000, -- price of the item if the shop allows buying (if not defined, it will set the Config.DefaultPrice value which is in config.lua)
    componentId = 5, -- (you can now overwrite the component Id) (by default its 7, but you can set the value here)
    clothing = {
        ["mp_m_freemode_01"] = {drawable = 82, texture = 0}, -- texture and drawable if its a male ped model
    },
    
    -- if offsets are commented, item wont have the option to add infused items
    
    -- offsets = { -- offsets for different infused items particle effects
    --     ["diamond"] = vec3( 0.0, 0.08, 0.34), -- offset for diamond
    --     ["ruby"] = vec3(0.0, 0.14, 0.36),   -- offset for ruby
    -- },
    
    craftItems = {
        ["diamond"] = {
            label = "Diamond",
            amount = 1,
        },
        ["ruby"] = {
            label = "Ruby",
            amount = 1,
        },
    }
},
```
# Difference between chains and watches

- Chains handle all component variations for peds. All the list of available component variations can be found here - https://docs.fivem.net/natives/?_0x262B14F48D29DE80
- Watches handle all prop variations for ped. All the list of available prop variations can be found here - https://docs.fivem.net/natives/?_0x93376B65A266EB5F

- So if you want to create a mask item, you will create an item and add the data for it in chains.lua since its a component variation with componentId 1.
- Similarly if you want to create a hat item, you will create an item and add the data for it in watches.lua since its a prop variation with componentId 0.


# Optional changes

- If you use chains as props, its advisable to remove chain when a player enters or leaves an apartment or a house. 
- Whenever player enter the house, just add this exports `exports["snipe-icebox"]:RemoveChains()` and the same export when they leave the house so there is no tornado effect when they sit in the car. 

# Known Issues

- If you are unable to see the ui at position but if you press E, it opens the context menu or safe, just set the `Config.UseLibTextUI` to false. Read the comments on that line
- The particle effects are not showing. You probably didnt set the offset properly in Config.Chains for that effect or that effect doesnt work. Always test the base effects given first to see if the effects are visible with new infuse items.