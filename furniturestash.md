# Framework

- This script only works with QBCore/ESX and support 3 inventories. Ox Inventory, QB Inventory and QS Inventory

# Config

- Make sure to read the config for all possible options.

# Items

## Ox Inventory

```lua
["furniture_stash_box"] = {
		label = "Furniture Stash Box",
		weight = 0,
		stack = true,
		close = true,
		consume = 0,
		description = "A stash box to store your stuff in",
		server = {
			export = "snipe-furniturestash.placeStash"
		}
	}
```

## QB inventory (PS/LJ)

```lua
["furniture_stash_box"] = {["name"] = "furniture_stash_box", ["label"] = "Furniture Stash Box", ["weight"] = 0, ["type"] = "item", ["image"] = "furniture_stash_box.png", ["unique"] = true, ["useable"] = true, ["shouldClose"] = true, ["combinable"] = nil, ["description"] = "A stash box to store your stuff in"}
```

## QS inventory 

```lua
["furniture_stash_box"] = {["name"] = "furniture_stash_box", ["label"] = "Furniture Stash Box", ["weight"] = 0, ["type"] = "item", ["image"] = "furniture_stash_box.png", ["unique"] = true, ["useable"] = true, ["shouldClose"] = true, ["combinable"] = nil, ["description"] = "A stash box to store your stuff in"}
```

# SQL

- Install the sql file present in the sql folder

# Add new stashes

- To add new stashes, you have to create a new item in your inventory. Depending on the inventory, follow the structure above. If you ox, you will need the export the same way I have added above
- Create an entry in Config.FurnitureStash in config.lua of the script. Make sure the item name is right and set the size/slot/price etc

# FAQ

- This system only works with my motel system. If the motel system is not started before the furniture stash, it will not work. Make sure to start the motel system before the furniture stash.