# Dependencies: 
- ox_inventory
- screenshot-basic
- fivemanage or fivemerr api key to store images

# Commands
- /evidence to open evidence menu near the designated locations
- /clearnearbyscene to clear all the evidence in the nearby area

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
		allowArmed = true,
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

	["bleach"] = {
		label = "Bleach",
		weight = 200,
		stack = false,
		close = false,
		description = "Clean up all the blood stains with this",
	},

	["evidence_tweezers"] = {
		label = "Tweezers",
		weight = 200,
		stack = false,
		close = false,
		description = "You can pick up small items with this",
	},

	["bactester"] = {
		label = "Bac Tester",
		weight = 200,
		stack = true,
		close = true,
		description = "Tool to get check BAC levels",
		client = {
			image = "bactester.png",
            export = "snipe-evidence.UseBacItem"
		},
		consume = 0,
	},

	["gsrkit"] = {
		label = "GSR Kit",
		weight = 200,
		stack = true,
		close = true,
		description = "Tool to get check GSR",
		client = {
			image = "gsrkit.png",
            export = "snipe-evidence.CheckGSR"
		},
		consume = 0,
	},

	["fingerprint_scanner"] = {
		label = "Fingerprint Scanner",
		weight = 200,
		stack = true,
		close = true,
		description = "A kit to take register fingerprints",
		client = {
			image = "fingerprint_scanner.png",
		},
		server = {
			export = "snipe-evidence.TakeFingerPrint"
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

## Why are the images for evidences not showing in inventory?
- You have a discord webhook set in your inventory.cfg/oxinventory.cfg file. Set it to blank and it will work.

## Why my gun auto holsters when I shoot?
- If a weapon is spawned directly in hand without using the item from inventory (probably deathmatch, arena, paintball scripts), it will auto holster. You can add exports for when player is in such areas so it doesnt auto holster.
- Look for function IgnoreEvidence in snipe-evidence/client/open/cl_customise.lua and add the exports there

- `set inventory:webhook ""`

# Developer Exports

## Client Side
```lua
exports['snipe-evidence']:isInrecreateMenu() -- returns true/fale if you are in recreate mode
exports['snipe-evidence']:OpenEvidenceUI() -- to open evidence ui using exports
exports["snipe-evidence"]:CreateFingerPrint(coords) -- to create fingerprint at the coords (coords is a vector3, if not passed, it will use player's current coords)
exports["snipe-evidence"]:IsWearingGloves() -- returns true/false if player is wearing gloves

exports["snipe-evidence"]:GenerateBloopDrop(coords) -- to generate bloop drop at coords from a person (will attach players dna to this evidence)
exports["snipe-evidence"]:CreateVehicleFingerprint(vehicle) -- creates fingerprint evidence for the vehicle (if vehicle is not passed it will not do anything)

exports["snipe-evidence"]:AddBacLevel(level) -- how much bac level you want to add (requires Config.BAC enabled)
exports["snipe-evidence"]:RemoveBacLevel(level) -- how much bac level you want to remove (requires Config.BAC enabled)

-- this is for the 3d text in camera (look at the usage)
exports["snipe-evidence"]:Camera3DTextExport() -- starts a thread in evidence system which shows the 3d text for evidences (should be triggered only once when you enable camera)
exports["snipe-evidence"]:Toggle3DText() -- toggles the 3d text to show the 3d texts
exports["snipe-evidence"]:StopCamera3DTextThread() -- stops the thread started in Camera3DTextExport()
```

## Server Side
```lua
exports["snipe-evidence"]:GetPlayerFingerprintInformation(source) -- returns name and fingerprint (check usage in server/open/sv_exports.lua)
exports["snipe-evidence"]:GetPlayerInformationFromFingerprint(fingerprint) -- returns name (check usage in server/open/sv_exports.lua)
exports["snipe-evidence"]:RemoveAllEvidences() -- removes all evidence present in the world
```

## Usage for the camera exports 
- this is just for test purposes and needs to be implemented by customer itself to make it work! No support provided to integrate in your personal scripts!!

```lua
CreateThread(function()
    local testEnable = true
    exports["snipe-evidence"]:Camera3DTextExport()
    while testEnable do
        Wait(0)
        if IsControlJustPressed(0, 38) then -- press E to toggle 3D text
            Toggle3DText()
        end

        if IsControlJustPressed(0, 177) then -- press ESC to stop the camera
            StopCamera3DTextThread()
            testEnable = false
        end
    end
end)
```

## Events

# When player enters recreate menu
```lua
AddEventHandler('snipe-evidence:client:enteredRecreate', function() end)
```

# When player leaves recreate menu
```lua
AddEventHandler('snipe-evidence:client:leftRecreate', function() end)
```