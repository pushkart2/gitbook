---
icon: material/printer
---

# Printer

Print real documents using an in-game printer/photocopy prop. Add a printer at any location by adding coords to `config.lua`.

---

## :material-cog: Configuration

```lua
Config.Printer = {
    [1] = { coords = vector3(-42.3, -1749.29, 29.42), heading = 320.32, SpawnModel = true, count = 50, capacity = 100 },

    -- coords      vector3 location
    -- heading     facing direction
    -- SpawnModel  whether to spawn the printer prop at the coords
    -- capacity    max A4 sheets the printer can hold
    -- count       initial A4-sheet count when the resource starts
}
```

## :material-package-variant-closed: Dependencies

- ESX or QBCore
- `ox_lib`
- One of: `linden_inventory`, `ox_inventory`, `quasar inventory`, or `qb-inventory`
- *Optional:* [interact-sound](https://github.com/qbcore-framework/interact-sound) — for printing sounds. The `.ogg` file is in `assets/`.

## :material-package-down: Installation

Drop assets from `assets/` into the right folders:

| File | Destination |
|---|---|
| `printer.ogg` | `interact-sound/client/html/sounds/` |
| `a4sheets.png` | `linden_inventory/html/images/` |
| `printerdocument.png` | `linden_inventory/html/images/` |

## :material-package-variant: Items

=== "Linden Inventory"

    Add to `linden_inventory/shared/items.lua`:

    ```lua
    ['a4sheets'] = {
        label  = 'A4 Sheets',
        weight = 500,
        stack  = true,
        close  = false,
    },

    ['printerdocument'] = {
        label  = 'Printed Document',
        weight = 1000,
        stack  = false,
        close  = true,
        client = {}
    },
    ```

    Then in `linden_inventory/client/main.lua`, find `linden_inventory:useItem` and add `and` to the existing condition:

    ```lua
    RegisterNetEvent('linden_inventory:useItem')
    AddEventHandler('linden_inventory:useItem', function(item)
        if item ~= "printerdocument" and item.metadata.bag and not currentInventory then
            invOpen = false
            TriggerServerEvent('linden_inventory:openInventory', 'bag', { id = item.metadata.bag, label = item.label..' ('..item.metadata.bag..')', slot = item.slot, slots = item.metadata.slot or 5 })
            return
        end
    end)
    ```

    Set `Config.Inventory = "linden"` in config.

=== "Ox Inventory"

    Set `Config.Inventory = "ox"` in config and add to `items.lua`:

    ```lua
    ['a4sheets'] = {
        label  = 'A4 Sheets',
        weight = 500,
        stack  = true,
        close  = false,
    },

    ['printerdocument'] = {
        label       = 'Printed Document',
        weight      = 1000,
        stack       = false,
        close       = true,
        description = nil,
        client      = { export = "snipe-printer.useDocument" },
    },
    ```

=== "Quasar Inventory"

    Set `Config.Inventory = "qs"` in config and add to `qs-core/config/config_items.lua`:

    ```lua
    ["printerdocument"] = {
        ["name"]        = "printerdocument",
        ["label"]       = "Printer Document",
        ["weight"]      = 50,
        ["type"]        = "item",
        ["image"]       = "printerdocument.png",
        ["unique"]      = true,
        ["useable"]     = true,
        ["shouldClose"] = true,
        ["combinable"]  = nil,
        ["description"] = "Important Document"
    },
    ["a4sheets"] = {
        ["name"]        = "a4sheets",
        ["label"]       = "A4 Sheets",
        ["weight"]      = 50,
        ["type"]        = "item",
        ["image"]       = "a4sheets.png",
        ["unique"]      = true,
        ["useable"]     = true,
        ["shouldClose"] = true,
        ["combinable"]  = nil,
        ["description"] = "Blank Papers to insert into printer"
    },
    ```

=== "QB Inventory"

    Set `Config.Inventory = "qb"` and add to `items.lua`:

    ```lua
    ['printerdocument'] = { ['name'] = 'printerdocument', ['label'] = 'Document',     ['weight'] = 500,  ['type'] = 'item', ['image'] = 'printer_documents.png', ['unique'] = true,  ['useable'] = true,  ['shouldClose'] = true, ['combinable'] = nil, ['description'] = 'A nice document' },
    ["a4sheets"]       = { ["name"] = "a4sheets",        ["label"] = "A4Sheets Pack", ["weight"] = 2000, ["type"] = "item", ["image"] = "a4sheets.png",          ["unique"] = false, ["useable"] = false, ["shouldClose"] = true, ["combinable"] = nil, ["description"] = "A bundle of 20 A4 Sheets" },
    ```

    Then patch `FormatItemInfo()` in `qb-inventory/html/js/app.js`:

    ```javascript
    function FormatItemInfo(itemData) {

        // Add this else-if for printerdocument
        else if (itemData.name == "printerdocument") {
            $(".item-info-title").html('<p>'+itemData.label+'</p>')
            $(".item-info-description").html('<p>'+itemData.info.documentname+'</p><br/>');
        }

    }
    ```

!!! tip
    For installation help, [join the Discord](https://discord.com/invite/VGYkkAYVv2).
