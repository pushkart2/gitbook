---
icon: material/bottle-tonic
---

# Moonshine

Distill, age, bottle, and sell moonshine — barrel-based fermentation with quality scoring.

---

## :material-package-variant: Items

=== "Ox Inventory"

    ```lua
    ["moonshine_bottle"] = {
        label       = "Moonshine Bottle",
        weight      = 1000,
        stack       = false,
        close       = true,
        description = "A bottle of moonshine",
        buttons     = {
            { label = 'Get Dropoff Location', action = function(slot) exports["snipe-moonshine"]:GetDropOffLocation(slot) end },
            { label = 'Drink',                action = function(slot) exports["snipe-moonshine"]:DrinkMoonshine(slot) end },
            { label = 'Drop Off',             action = function(slot) exports["snipe-moonshine"]:DropOff(slot) end },
        },
    },
    ["moonshine_barrel"] = {
        label       = "Moonshine Barrel",
        weight      = 10000,
        stack       = true,
        close       = true,
        consume     = 0,
        server      = { export = "snipe-moonshine.useBarrel" },
        description = "A barrel of moonshine",
    },
    ["moonshine_crate"] = {
        label       = "Moonshine Crate",
        weight      = 10000,
        stack       = true,
        close       = true,
        description = "A crate of moonshine",
    },

    ["mold_corn"]   = { label = "Moldy Corn",   weight = 100, stack = true, close = false, description = "A bag of moldy corn"   },
    ["mold_apple"]  = { label = "Moldy Apple",  weight = 100, stack = true, close = false, description = "A bag of moldy apple"  },
    ["mold_bread"]  = { label = "Moldy Bread",  weight = 100, stack = true, close = false, description = "A bag of moldy bread"  },
    ["mold_barley"] = { label = "Moldy Barley", weight = 100, stack = true, close = false, description = "A bag of moldy barley" },
    ```

=== "QB / QS Inventory"

    ```lua
    ["moonshine_bottle"] = { name = "moonshine_bottle", label = "Moonshine Bottle", weight = 100,  type = 'item', image = 'moonshine_bottle.png', unique = true,  useable = true,  shouldClose = true, combinable = nil, description = 'A Moonshine Bottle' },
    ["moonshine_barrel"] = { name = "moonshine_barrel", label = "Moonshine Barrel", weight = 1000, type = 'item', image = 'moonshine_barrel.png', unique = true,  useable = true,  shouldClose = true, combinable = nil, description = 'A Moonshine Barrel' },
    ["moonshine_crate"]  = { name = "moonshine_crate",  label = "Moonshine Crate",  weight = 1000, type = 'item', image = 'moonshine_crate.png',  unique = true,  useable = false, shouldClose = true, combinable = nil, description = 'A Moonshine Crate' },
    ["mold_corn"]        = { name = "mold_corn",        label = "Mold Corn",        weight = 1000, type = 'item', image = 'mold_corn.png',        unique = false, useable = false, shouldClose = true, combinable = nil, description = 'A moldy corn' },
    ["mold_apple"]       = { name = "mold_apple",       label = "Mold Apple",       weight = 1000, type = 'item', image = 'mold_apple.png',       unique = false, useable = false, shouldClose = true, combinable = nil, description = 'A moldy apple' },
    ["mold_bread"]       = { name = "mold_bread",       label = "Mold Bread",       weight = 1000, type = 'item', image = 'mold_bread.png',       unique = false, useable = false, shouldClose = true, combinable = nil, description = 'A moldy Bread' },
    ["mold_barley"]      = { name = "mold_barley",      label = "Mold Barley",      weight = 1000, type = 'item', image = 'mold_barley.png',      unique = false, useable = false, shouldClose = true, combinable = nil, description = 'A moldy Barley' },
    ["water"]            = { name = "water",            label = "Water",            weight = 1000, type = 'item', image = 'water.png',            unique = false, useable = false, shouldClose = true, combinable = nil, description = 'A bottle of water' },
    ```

## :material-database: SQL

Run the SQL file from the `Install Me SQL` folder.

## :material-cog: Configuration

Read every comment in the `shared/` folder — each line is documented.

## :material-tune: Optional — quality metadata in QB inventory UI

!!! warning "Out-of-scope support"
    No support is provided for the various qb / ps / lj inventory forks. The snippet below targets the **latest qb-inventory only** — adapting it to forks is your responsibility. Don't open a ticket for this.

In `qb-inventory/html/js/app.js` ([reference](https://github.com/qbcore-framework/qb-inventory/blob/main/html/js/app.js#L336)), find `function generateDescription(itemData)` and add the `moonshine_bottle` case:

```js
case "labkey": // existing
    return `<p>Lab: ${itemData.info.lab}</p>`; // existing
case "moonshine_bottle": // add
    return `<p><strong>Quality: </strong><span>${itemData.info.moonshine_quality_label}</span></p>`; // add
default: // existing
```
