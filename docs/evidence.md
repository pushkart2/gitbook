---
icon: material/fingerprint
---

# Evidence System

A complete evidence collection workflow — bagging, photography, DNA/GSR/BAC analysis, fingerprints, and custom evidence types.

---

## :material-package-variant-closed: Dependencies

- `ox_inventory`
- `screenshot-basic`
- A [fivemanage](https://fivemanage.com/) or [fivemerr](https://fivemerr.com/) API key for image storage

## :material-console: Commands

| Command | Action |
|---|---|
| `/evidence` | Open the evidence menu near a designated location |
| `/clearnearbyscene` | Clear all evidence in the nearby area |

## :material-package-variant: Step 1 — Items

Add the items below to ox_inventory. Item images are in `snipe-evidence/images/`.

```lua
["collected_evidence_bag"] = {
    label       = "Evidence Bag",
    weight      = 200,
    stack       = false,
    close       = false,
    description = "A filled evidence bag",
},

["evidence_camera"] = {
    label       = "Evidence Camera",
    weight      = 200,
    stack       = false,
    close       = true,
    description = "Camera to take pictures of evidence",
    client      = { image = "evidence_camera.png" },
    server      = { export = "snipe-evidence.useCam" },
    consume     = 0,
},

["evidence_pouch"] = {
    label       = "Evidence Pouch",
    weight      = 200,
    stack       = false,
    close       = false,
    server      = { export = 'snipe-evidence.usePouch' },
    client      = { image = "evidence_pouch.png" },
    description = "Pouch to hold all your evidences",
    consume     = 0,
    allowArmed  = true,
},

["dna_swab_kit"] = {
    label       = "DNA Swab Kit",
    weight      = 200,
    stack       = true,
    close       = true,
    description = "A kit to take DNA samples",
    client      = { image = "dna_swab_kit.png" },
    server      = { export = "snipe-evidence.swabDNA" },
    consume     = 0,
},

["accesstool"] = {
    label       = "Access Tool",
    weight      = 200,
    stack       = true,
    close       = true,
    description = "Tool to get into locked cars",
    client      = { image = "accesstool.png" },
    server      = { export = "snipe-evidence.useAccessTool" },
    consume     = 0,
},

["bleach"] = {
    label       = "Bleach",
    weight      = 200,
    stack       = false,
    close       = false,
    description = "Clean up all the blood stains with this",
},

["evidence_tweezers"] = {
    label       = "Tweezers",
    weight      = 200,
    stack       = false,
    close       = false,
    description = "You can pick up small items with this",
},

["bactester"] = {
    label       = "Bac Tester",
    weight      = 200,
    stack       = true,
    close       = true,
    description = "Tool to check BAC levels",
    client      = {
        image  = "bactester.png",
        export = "snipe-evidence.UseBacItem"
    },
    consume     = 0,
},

["gsrkit"] = {
    label       = "GSR Kit",
    weight      = 200,
    stack       = true,
    close       = true,
    description = "Tool to check GSR",
    client      = {
        image  = "gsrkit.png",
        export = "snipe-evidence.CheckGSR"
    },
    consume     = 0,
},

["fingerprint_scanner"] = {
    label       = "Fingerprint Scanner",
    weight      = 200,
    stack       = true,
    close       = true,
    description = "A kit to register fingerprints",
    client      = { image = "fingerprint_scanner.png" },
    server      = { export = "snipe-evidence.TakeFingerPrint" },
    consume     = 0,
},

["dnasample"] = {
    label       = "DNA Sample",
    weight      = 200,
    stack       = false,
    close       = false,
    description = "DNA sample gathered using the swab kit",
},

["evidence_objects"] = {
    label       = "Evidence Objects",
    weight      = 200,
    stack       = true,
    close       = false,
    consume     = 0,
    server      = { export = "snipe-evidence.useObjects" },
    description = "Objects that can be placed around crime scenes",
},
```

## :material-cog: Step 2 — Configuration

Tune the script in:

| File | Purpose |
|---|---|
| `shared/config.lua` | Main config |
| `shared/locales.lua` | Translations |
| `shared/progress.lua` | Progress-bar timings |

Read the comments — only change values that are explicitly documented.

## :material-police-badge: Step 3 — Other police-job scripts

=== "qb-policejob"

    Comment out the entire `qb-policejob/client/evidence.lua`. Leaving it active won't break anything but officers will see duplicated evidence from both scripts.

=== "Any other evidence system"

    Remove it completely. You can also delete its items — they won't be used anymore.

## :material-help-circle: FAQ

??? question "I can't see images in the evidence UI when I click the camera icon"
    Your image API key isn't set up correctly in `server/open/sv_image_api.lua`.

??? question "Evidence images aren't showing in inventory"
    You have a Discord webhook set in `inventory.cfg` / `oxinventory.cfg`. Clear it:

    ```cfg
    set inventory:webhook ""
    ```

??? question "My gun auto-holsters when I shoot"
    A weapon spawned directly into the player's hand (without using an inventory item) will auto-holster — common in deathmatch / arena / paintball scripts.

    Add an exception in `IgnoreEvidence` (in `snipe-evidence/client/open/cl_customise.lua`) for those zones.

??? question "Do I get items when using the fingerprint scanner or DNA swab kit?"
    No — the player's fingerprint/DNA is registered in the system. When you search for that fingerprint in the UI, the player's name shows up instead of "No Result Found".

## :material-code-tags: Developer exports

### Client

```lua
exports['snipe-evidence']:isInrecreateMenu()                 -- true if in recreate mode
exports['snipe-evidence']:OpenEvidenceUI()                   -- open evidence UI
exports["snipe-evidence"]:CreateFingerPrint(coords)          -- creates a fingerprint at coords (default: player coords)
exports["snipe-evidence"]:IsWearingGloves()                  -- true if player is wearing gloves

exports["snipe-evidence"]:GenerateBloopDrop(coords)          -- generate a blood-drop with attached DNA at coords
exports["snipe-evidence"]:CreateVehicleFingerprint(vehicle)  -- fingerprint evidence for a vehicle (no-op if vehicle nil)

exports["snipe-evidence"]:AddBacLevel(level)                 -- add BAC (requires Config.BAC enabled)
exports["snipe-evidence"]:RemoveBacLevel(level)              -- remove BAC

-- 3D text on camera evidence (see usage example below)
exports["snipe-evidence"]:Camera3DTextExport()               -- start the 3D-text thread (call once when enabling camera)
exports["snipe-evidence"]:Toggle3DText()                     -- toggle visibility
exports["snipe-evidence"]:StopCamera3DTextThread()           -- stop the thread
```

### Server

```lua
exports["snipe-evidence"]:GetPlayerFingerprintInformation(source)  -- returns name + fingerprint
exports["snipe-evidence"]:GetPlayerInformationFromFingerprint(fp)  -- returns name from fingerprint
exports["snipe-evidence"]:RemoveAllEvidences()                     -- removes all evidence in the world
```

### Camera 3D-text usage example

!!! warning "Reference only"
    This example is for testing — integrate the pattern into your own script. **No support is provided for custom integrations.**

```lua
CreateThread(function()
    local testEnable = true
    exports["snipe-evidence"]:Camera3DTextExport()

    while testEnable do
        Wait(0)
        if IsControlJustPressed(0, 38) then -- E to toggle
            Toggle3DText()
        end
        if IsControlJustPressed(0, 177) then -- ESC to stop
            StopCamera3DTextThread()
            testEnable = false
        end
    end
end)
```

### Events

```lua
-- when player enters recreate menu
AddEventHandler('snipe-evidence:client:enteredRecreate', function() end)

-- when player leaves recreate menu
AddEventHandler('snipe-evidence:client:leftRecreate', function() end)
```

## :material-flask: Custom evidence

Add custom evidence types in `shared/config.lua`. Read an existing example first — every required field must be populated or the evidence will not register.

```lua
-- get the player's DNA + fingerprint to attach to the evidence
local data = exports["snipe-evidence"]:GetPlayerInfo()
-- returns { dna = dnaid, fingerprint = fingeprintid }

-- create custom evidence
-- (if any field configured in Config is missing, creation fails — check F8 prints for details)
exports["snipe-evidence"]:CreateCustomEvidence("shoeprint", {
    shoetype = "Test Shoe Type",
    shoesize = "Test Shoe Size"
})
```
