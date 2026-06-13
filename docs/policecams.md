---
icon: material/cctv
---

# Police Cams

Bodycams, dashcams, bait cars, and a police tablet for live camera viewing.

---

## :material-package-down: Installation

1. Drop the resource into `resources/`. Make sure it starts at the **end** of `server.cfg`.
2. Read the config and tune to your server's needs.
3. Item images are in `assets/` (only needed if you go item-based).

## :material-package-variant: Item-based

=== "QBCore"

    Add to `qb-core/shared/items.lua`:

    ```lua
    ["bodycam"]      = { ["name"] = "bodycam",      ["label"] = "Bodycam",       ["weight"] = 2000, ["type"] = "item", ["image"] = "bodycam.png",      ["unique"] = false, ["useable"] = true, ["shouldClose"] = true, ["combinable"] = nil, ["description"] = "Bodycam" },
    ["dashcam"]      = { ["name"] = "dashcam",      ["label"] = "Dashcam",       ["weight"] = 2000, ["type"] = "item", ["image"] = "dashcam.png",      ["unique"] = false, ["useable"] = true, ["shouldClose"] = true, ["combinable"] = nil, ["description"] = "Dashcam" },
    ["policetablet"] = { ["name"] = "policetablet", ["label"] = "Police Tablet", ["weight"] = 2000, ["type"] = "item", ["image"] = "policetablet.png", ["unique"] = false, ["useable"] = true, ["shouldClose"] = true, ["combinable"] = nil, ["description"] = "Police Tablet" },
    ["gpsmodule"]    = { ["name"] = "gpsmodule",    ["label"] = "GPS Module",    ["weight"] = 2000, ["type"] = "item", ["image"] = "gpsmodule.png",    ["unique"] = false, ["useable"] = true, ["shouldClose"] = true, ["combinable"] = nil, ["description"] = "GPS Module to setup Bait Cars" },
    ```

=== "ESX"

    ```sql
    INSERT INTO `items` (`name`, `label`, `weight`, `rare`, `can_remove`) VALUES ('bodycam',      'Bodycam',       1, 0, 1);
    INSERT INTO `items` (`name`, `label`, `weight`, `rare`, `can_remove`) VALUES ('dashcam',      'Dashcam',       1, 0, 1);
    INSERT INTO `items` (`name`, `label`, `weight`, `rare`, `can_remove`) VALUES ('policetablet', 'Police Tablet', 1, 0, 1);
    INSERT INTO `items` (`name`, `label`, `weight`, `rare`, `can_remove`) VALUES ('gpsmodule',    'GPS Module',    1, 0, 1);
    ```

=== "OX Inventory"

```lua

['bodycam'] = { 
		label = 'Body Cam', 
		weight = 300, 
		stack = false, 
		close = true, 
		description = 'A wearable camera used by law enforcement to record interactions',
		client = { image = 'bodycam.png' }, 
		consume = 0,
		server = {
			export = "snipe-policecams.useBodycam"
		}
	},

	['dashcam'] = { 
	label = 'Dash Cam', 
	weight = 500, 
	stack = false, 
	close = true, 
	description = 'A vehicle-mounted camera that records the road ahead', 
	client = { image = 'dashcam.png' }, 
	consume = 0,
	server = {
		export = "snipe-policecams.useDashcam"
	}
},

	['gpsmodule'] = { 
		label = 'GPS Module', 
		weight = 200, 
		stack = true, 
		close = false, 
		description = 'A tracking device used to monitor vehicle or player location', 
		client = { image = 'gpsmodule.png' }, 
		consume = 0,
		server = {
			export = "snipe-policecams.useGpsModule"
		}
	},

	["policetablet"] = {
		label = "Police Tablet",
		weight = 300,
		stack = false,
		close = true,
		description = "A tablet used by police officers to access various functions and information",
		client = { image = 'policetablet.png' },
		consume = 0,
		server = {
			export = "snipe-policecams.useTablet"
		}
	},
```

## :material-console: Command-based

| Command | Action |
|---|---|
| `/bodycam` | Toggle bodycam on/off |
| `/dashcam` | Install dashcam |
| `/baitcar` | Set up a bait car |
| `/cams` | View all active cameras |
| `/removedashcam` | Remove dashcam |
| `/removebaitcar` | Remove bait car |
