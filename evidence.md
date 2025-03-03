# Dependencies: 
- ox_inventory
- screenshot-basic
- fivemanage or fivemerr api key to store images

# Install Items To Ox Inventory and copy Images
- Install the following items
- All the images are present in snipe-evidence/images folder

```lua
    ["collected_evidence_bag"] = {
		label = "Evidence Bag",
		weight = 200,
		stack = false,
		close = false,
		description = "A filled evidence bag",
	},

	["evidence_camera"] = {
		label = "Evidence Camera",
		weight = 200,
		stack = false,
		close = true,
		description = "Camera to take pictures of evidence",
		client = {
			image = "evidence_camera.png",
		},
		server = {
			export = "snipe-evidence.useCam"
		},
		consume = 0,
	},

	["evidence_pouch"] = {
		label = "Evidence Pouch",
		weight = 200,
		stack = false,
		close = false,
		server = {
			export = 'snipe-evidence.usePouch'
		},
		client = {
			image = "evidence_pouch.png",
		},
		description = "Pouch to hold all your evidences",
		consume = 0,
	},

	["dna_swab_kit"] = {
		label = "DNA Swab Kit",
		weight = 200,
		stack = true,
		close = true,
		description = "A kit to take DNA samples",
		client = {
			image = "dna_swab_kit.png",
		},
		server = {
			export = "snipe-evidence.swabDNA"
		},
		consume = 0,
	},

	["accesstool"] = {
		label = "Access Tool",
		weight = 200,
		stack = true,
		close = true,
		description = "Tool to get into locked cars",
		client = {
			image = "accesstool.png",
		},
		server = {
			export = "snipe-evidence.useAccessTool"
		},
		consume = 0,
	},
```

# Configuration.
- Go through shared/config.lua, shared/locales.lua and shared/progress.lua to make adjustment to your liking.
- Make sure to read the comments and only change what is mentioned.

# For QB-Policejob
- Comment out the whole evidence.lua file in qb-policejob/client folder. 
- If you dont comment, it wont mess with my script but police will see evidence from qb-policejob and my script and it will be confusing.

# Any other evidence System
- If you use any other evidence system, make sure to remove it completely.
- You can also remove all the items pertaining to that since they wont be used anymore.

# FAQ

## Why cant I see images in the evidence ui by clicking on camera icon?
- You have not setup the api correctly in the server/open/sv_image_api.lua