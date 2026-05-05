---
icon: material/face-woman
---

# Wigs

A disguise system using wigs and hairstyles, with crafting and dyeing.

---

## :material-package-down: Installation

1. Install all the items below for your inventory.
2. Run the SQL file from the `SQL-Install-Me` folder.
3. Copy item icons from `assets/` into your inventory's images folder.
4. In `shared/config.lua`, set the job, inventory, and clothing-resource names per the inline comments.
5. In `shared/locations.lua`, configure coords for wig testing and crafting, plus the items required to craft each wig.

## :material-package-variant: Items

=== "Ox Inventory"

    ```lua
    ['wig_hair_bundle'] = { label = 'Wig Hair Bundle' },
    ['wig_dye']         = { label = 'Wig Dye' },
    ['wig_cap']         = { label = 'Wig Cap' },

    ["wig_glue"] = {
        label  = "Wig Glue",
        weight = 1,
        stack  = true,
    },

    ["wig"] = {
        label   = "Wig",
        weight  = 1,
        stack   = false,
        server  = { export = 'snipe-wigs.useWig' },
        consume = 0,
    }
    ```

=== "QB Inventory"

    ```lua
    ['wig'] = {
        ['name'] = 'wig', ['label'] = 'Wig', ['weight'] = 150, ['type'] = 'item',
        ['image'] = 'wig.png', ['unique'] = true, ['useable'] = true,
        ['shouldClose'] = false, ['combinable'] = nil, ['description'] = 'none'
    },

    ['wig_hair_bundle'] = {
        ['name'] = 'wig_hair_bundle', ['label'] = 'Wig Hair Bundle', ['weight'] = 150, ['type'] = 'item',
        ['image'] = 'wig_hair_bundle.png', ['unique'] = false, ['useable'] = false,
        ['shouldClose'] = false, ['combinable'] = nil, ['description'] = 'none'
    },

    ['wig_dye'] = {
        ['name'] = 'wig_dye', ['label'] = 'Wig Dye', ['weight'] = 150, ['type'] = 'item',
        ['image'] = 'wig_dye.png', ['unique'] = false, ['useable'] = false,
        ['shouldClose'] = false, ['combinable'] = nil, ['description'] = 'none'
    },

    ['wig_cap'] = {
        ['name'] = 'wig_cap', ['label'] = 'Wig Cap', ['weight'] = 150, ['type'] = 'item',
        ['image'] = 'wig_cap.png', ['unique'] = false, ['useable'] = false,
        ['shouldClose'] = false, ['combinable'] = nil, ['description'] = 'none'
    },

    ['wig_glue'] = {
        ['name'] = 'wig_glue', ['label'] = 'Wig Glue', ['weight'] = 150, ['type'] = 'item',
        ['image'] = 'wig_glue.png', ['unique'] = false, ['useable'] = false,
        ['shouldClose'] = false, ['combinable'] = nil, ['description'] = 'none'
    },
    ```

=== "QS Inventory"

    ```lua
    ['wig'] = {
        ['name'] = 'wig', ['label'] = 'Wig', ['weight'] = 150, ['type'] = 'item',
        ['image'] = 'wig.png', ['unique'] = true, ['useable'] = true,
        ['shouldClose'] = false, ['combinable'] = nil, ['description'] = 'none'
    },

    ['wig_hair_bundle'] = {
        ['name'] = 'wig_hair_bundle', ['label'] = 'Wig Hair Bundle', ['weight'] = 150, ['type'] = 'item',
        ['image'] = 'wig_hair_bundle.png', ['unique'] = false, ['useable'] = false,
        ['shouldClose'] = false, ['combinable'] = nil, ['description'] = 'none'
    },

    ['wig_dye'] = {
        ['name'] = 'wig_dye', ['label'] = 'Wig Dye', ['weight'] = 150, ['type'] = 'item',
        ['image'] = 'wig_dye.png', ['unique'] = false, ['useable'] = false,
        ['shouldClose'] = false, ['combinable'] = nil, ['description'] = 'none'
    },

    ['wig_cap'] = {
        ['name'] = 'wig_cap', ['label'] = 'Wig Cap', ['weight'] = 150, ['type'] = 'item',
        ['image'] = 'wig_cap.png', ['unique'] = false, ['useable'] = false,
        ['shouldClose'] = false, ['combinable'] = nil, ['description'] = 'none'
    },

    ['wig_glue'] = {
        ['name'] = 'wig_glue', ['label'] = 'Wig Glue', ['weight'] = 150, ['type'] = 'item',
        ['image'] = 'wig_glue.png', ['unique'] = false, ['useable'] = false,
        ['shouldClose'] = false, ['combinable'] = nil, ['description'] = 'none'
    },
    ```
