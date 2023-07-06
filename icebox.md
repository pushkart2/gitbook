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
	["ruby"]						 = {["name"] = "ruby", 						["label"] = "Ruby", 				["weight"] = 0, 		["type"] = "item", 		["image"] = "ruby.png", 				["unique"] = false, 	["useable"] = false, 	["shouldClose"] = true,	   		["combinable"] = nil,   ["description"] = "Ruby"},
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

    ['diamond'] = {
        ['name'] = 'clothing_chain',
        ['label'] = 'Diamond',
        ['weight'] = 500,
        ['type'] = 'item',
        ['image'] = 'clothing_chain.png',
        ['unique'] = true,
        ['useable'] = true,
        ['shouldClose'] = false,
        ['combinable'] = nil,
        ['description'] = 'none'
    },

    ['ruby'] = {
        ['name'] = 'clothing_chain',
        ['label'] = 'Ruby',
        ['weight'] = 500,
        ['type'] = 'item',
        ['image'] = 'clothing_chain.png',
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

# Clothing Changes

## Illenium-appearance

### Add this line `exports["snipe-icebox"]:RemoveChains()` below the following lines

- `RegisterNetEvent("illenium-appearance:client:changeOutfit", function(data)` in client/client.lua
- `RegisterNetEvent("illenium-appearance:client:reloadSkin", function(bypassChecks)` in client/client.lua
- `function client.startPlayerCustomization(cb, conf)` in game/customization.lua

## qb-clothing

### Add this line `exports["snipe-icebox"]:RemoveChains()` below the following lines

- `function openMenu(allowedMenus)` in client/main.lua
- `AddEventHandler('qb-clothing:client:loadOutfit', function(oData)` in client/main.lua

## Others

- If you use any other clothing skin, make sure to add the RemoveChains() export when you change your outfit, go into clothing menu, reload skin. 
- I mean these changes are not really required but if you use chain as clothing item and people save the outfit, the chain will be saved as well in the outfits. So its advisable to do that.
- I will try to add support for as many clothing scripts I can but if they are encrypted, you will have to reach out to author of the scripts to add the export.

# Optional changes

- If you use chains as props, its advisable to remove chain when a player enters or leaves an apartment or a house. 
- Whenever player enter the house, just add this exports `exports["snipe-icebox"]:RemoveChains()` and the same export when they leave the house so there is no tornado effect when they sit in the car. 
- If you intend to use only clothing as chains, you can ignore this but I would 100% advise to do the clothing changes.

# Known Issues

- If you are unable to see the ui at position but if you press E, it opens the context menu or safe, just set the `Config.UseLibTextUI` to false. Read the comments on that line
- The particle effects are not showing. You probably didnt set the offset properly in Config.Chains for that effect or that effect doesnt work. Always test the base effects given first to see if the effects are visible with new infuse items.