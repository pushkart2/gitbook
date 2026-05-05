---
icon: material/flask-outline
---

# Meth

Cook, bag, and distribute methamphetamine — table-based crafting with a slider mini-game and quality scoring.

---

## :material-package-down: Installation

- Configure the config based on your framework.
- Item images are in the `assets/` folder.

## :material-package-variant-closed: Requirements

- [SliderGame](https://github.com/pushkart2/slidergame) 
- A target system: **qb-target**, **qtarget**, or **ox_target**

## :material-package-variant: Items

=== "Ox Inventory"

    ```lua
    ['methtable'] = {
        label   = 'Meth Table',
        weight  = 220,
        stack   = false,
        consume = 0,
        client  = { export = 'snipe-meth.UseTable' }
    },
    ['methbag'] = {
        label   = 'Meth Bag',
        weight  = 220,
        stack   = false,
        consume = 0,
        client  = { export = 'snipe-meth.UseBag' }
    },
    ['crack_baggy'] = {
        label   = 'Bag of Crack',
        weight  = 220,
        stack   = true,
        consume = 0,
    },
    ```

=== "QB Inventory"

    ```lua
    ["methtable"] = { ["name"] = "methtable", ["label"] = "Meth Table", ["weight"] = 2000, ["type"] = "item", ["image"] = "methtable.png", ["unique"] = true, ["useable"] = true, ["shouldClose"] = true, ["combinable"] = nil, ["description"] = "A Table" },
    ["methbag"]   = { ["name"] = "methbag",   ["label"] = "Meth Bag",   ["weight"] = 1000, ["type"] = "item", ["image"] = "meth10g.png",   ["unique"] = true, ["useable"] = true, ["shouldClose"] = true, ["combinable"] = nil, ["description"] = "Meth Bag" },
    ```

    ### Inventory patches

    **`inventory/html/js/app.js`** — find `function FormatItemInfo` and add the two `else if` blocks for `methtable` and `methbag` (the `labkey` block already exists):

    ```js
    else if (itemData.name == "labkey") {
        $(".item-info-title").html("<p>" + itemData.label + "</p>");
        $(".item-info-description").html("<p>Lab: " + itemData.info.lab + "</p>");
    }
    else if (itemData.name == "methtable") {
        $(".item-info-title").html("<p>" + itemData.label + "</p>");
        $(".item-info-description").html("<p>Table ID: " + itemData.info.tableid + "</p>");
    }
    else if (itemData.name == "methbag") {
        $(".item-info-title").html("<p>" + itemData.label + "</p>");
        $(".item-info-description").html("<p>Quality: " + itemData.info.qualityIndex + "</p>");
    }
    ```

    **`inventory/server/main.lua`** *(optional — only if you spawn tables/bags via `/giveitem`)* — find `QBCore.Commands.Add("giveitem"` and add the two `elseif` blocks for `methtable` and `methbag`:

    ```lua
    elseif itemData["name"] == "labkey" then
        info.lab = exports["qb-methlab"]:GenerateRandomLab()
    elseif itemData["name"] == "methtable" then
        info.tableid = tostring(QBCore.Shared.RandomInt(5) .. QBCore.Shared.RandomStr(2) .. QBCore.Shared.RandomInt(6))
    elseif itemData["name"] == "methbag" then
        info.quality = math.random(1, 100)
    ```

=== "Quasar Inventory"

    ```lua
    ["methtable"] = {
        ["name"]        = "methtable",
        ["label"]       = "Meth Table",
        ["weight"]      = 2000,
        ["type"]        = "item",
        ["image"]       = "methtable.png",
        ["unique"]      = true,
        ["useable"]     = true,
        ["shouldClose"] = true,
        ["combinable"]  = nil,
        ["description"] = "A Table"
    },
    ["methbag"] = {
        ["name"]        = "methbag",
        ["label"]       = "Meth Bag",
        ["weight"]      = 1000,
        ["type"]        = "item",
        ["image"]       = "meth10g.png",
        ["unique"]      = true,
        ["useable"]     = true,
        ["shouldClose"] = true,
        ["combinable"]  = nil,
        ["description"] = "Meth Bag"
    },
    ["crack_baggy"] = {
        ["name"]        = "crack_baggy",
        ["label"]       = "Bag of Crack",
        ["weight"]      = 100,
        ["type"]        = "item",
        ["image"]       = "meth10g.png",
        ["unique"]      = false,
        ["useable"]     = true,
        ["shouldClose"] = true,
        ["combinable"]  = nil,
        ["description"] = "Small bag of crack"
    },
    ```
