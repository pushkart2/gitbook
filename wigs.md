# Steps for installation
1. Install all the items mentioned below based on your inventory
2. Grab the icons from asset folders and put them in your inventory images folder
3. Go to shared/config.lua, modify the job, inventory, clothing resource name, etc as per the comments mentioned there.
4. Go to shared/locations.lua to change/add coords for wig testing and wig crafting as well as the items required to craft the wigs.


# Install the following items based on your inventory

## Ox Inventory

```lua
['wig_hair_bundle'] = {
	label = 'Wig Hair Bundle',
},

['wig_dye'] = {
    label = 'Wig Dye',
},

['wig_cap'] = {
    label = 'Wig Cap',
},

["wig_glue"] = {
    label = "Wig Glue",
    weight = 1,
    stack = true,
},

["wig"] = {
    label = "Wig",
    weight = 1,
    stack = false,
    server = {
        export = 'snipe-wigs.useWig',
    },
    consume = 0,
}
```

## QS Inventory

```lua
['wig'] = {
    ['name'] = 'wig',
    ['label'] = 'Wig',
    ['weight'] = 150,
    ['type'] = 'item',
    ['image'] = 'wig.png',
    ['unique'] = true,
    ['useable'] = true,
    ['shouldClose'] = false,
    ['combinable'] = nil,
    ['description'] = 'none'
},

['wig_hair_bundle'] = {
    ['name'] = 'wig_hair_bundle',
    ['label'] = 'Wig Hair Bundle',
    ['weight'] = 150,
    ['type'] = 'item',
    ['image'] = 'bleach.png',
    ['unique'] = false,
    ['useable'] = false,
    ['shouldClose'] = false,
    ['combinable'] = nil,
    ['description'] = 'none'
},

['wig_dye'] = {
    ['name'] = 'wig_dye',
    ['label'] = 'Wig Dye',
    ['weight'] = 150,
    ['type'] = 'item',
    ['image'] = 'developer.png',
    ['unique'] = false,
    ['useable'] = false,
    ['shouldClose'] = false,
    ['combinable'] = nil,
    ['description'] = 'none'
},

['wig_cap'] = {
    ['name'] = 'wig_cap',
    ['label'] = 'Wig Cap',
    ['weight'] = 150,
    ['type'] = 'item',
    ['image'] = 'dry_bleached_hair.png',
    ['unique'] = false,
    ['useable'] = false,
    ['shouldClose'] = false,
    ['combinable'] = nil,
    ['description'] = 'none'
},

    ['wig_glue'] = {
    ['name'] = 'wig_glue',
    ['label'] = 'Wig Glue',
    ['weight'] = 150,
    ['type'] = 'item',
    ['image'] = 'dry_bleached_hair.png',
    ['unique'] = false,
    ['useable'] = false,
    ['shouldClose'] = false,
    ['combinable'] = nil,
    ['description'] = 'none'
},
```

## QB Inventory

```lua
['wig'] = {
    ['name'] = 'wig',
    ['label'] = 'Wig',
    ['weight'] = 150,
    ['type'] = 'item',
    ['image'] = 'wig.png',
    ['unique'] = true,
    ['useable'] = true,
    ['shouldClose'] = false,
    ['combinable'] = nil,
    ['description'] = 'none'
},

['wig_hair_bundle'] = {
    ['name'] = 'wig_hair_bundle',
    ['label'] = 'Wig Hair Bundle',
    ['weight'] = 150,
    ['type'] = 'item',
    ['image'] = 'bleach.png',
    ['unique'] = false,
    ['useable'] = false,
    ['shouldClose'] = false,
    ['combinable'] = nil,
    ['description'] = 'none'
},

['wig_dye'] = {
    ['name'] = 'wig_dye',
    ['label'] = 'Wig Dye',
    ['weight'] = 150,
    ['type'] = 'item',
    ['image'] = 'developer.png',
    ['unique'] = false,
    ['useable'] = false,
    ['shouldClose'] = false,
    ['combinable'] = nil,
    ['description'] = 'none'
},

['wig_cap'] = {
    ['name'] = 'wig_cap',
    ['label'] = 'Wig Cap',
    ['weight'] = 150,
    ['type'] = 'item',
    ['image'] = 'dry_bleached_hair.png',
    ['unique'] = false,
    ['useable'] = false,
    ['shouldClose'] = false,
    ['combinable'] = nil,
    ['description'] = 'none'
},

    ['wig_glue'] = {
    ['name'] = 'wig_glue',
    ['label'] = 'Wig Glue',
    ['weight'] = 150,
    ['type'] = 'item',
    ['image'] = 'dry_bleached_hair.png',
    ['unique'] = false,
    ['useable'] = false,
    ['shouldClose'] = false,
    ['combinable'] = nil,
    ['description'] = 'none'
},
```