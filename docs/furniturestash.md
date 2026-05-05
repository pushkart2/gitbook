---
icon: material/sofa
---

# Furniture Stash

Place furniture items as stashes inside motel rooms — works as an extension of the snipe-motel system.

---

!!! danger "Start order matters"
    `snipe-furniturestash` **must start after `snipe-motel`** in `server.cfg`. The stash system depends on the motel system being initialised first.

## :material-source-branch: Framework support

QBCore and ESX. Inventory support: **Ox Inventory**, **QB Inventory**, **QS Inventory**.

## :material-cog: Config

Read every comment in `config.lua` — it documents every option.

## :material-package-variant: Items

=== "Ox Inventory"

    ```lua
    ["furniture_stash_box"] = {
        label       = "Furniture Stash Box",
        weight      = 0,
        stack       = true,
        close       = true,
        consume     = 0,
        description = "A stash box to store your stuff in",
        server      = { export = "snipe-furniturestash.placeStash" }
    }
    ```

=== "QB / PS / LJ Inventory"

    ```lua
    ["furniture_stash_box"] = { ["name"] = "furniture_stash_box", ["label"] = "Furniture Stash Box", ["weight"] = 0, ["type"] = "item", ["image"] = "furniture_stash_box.png", ["unique"] = true, ["useable"] = true, ["shouldClose"] = true, ["combinable"] = nil, ["description"] = "A stash box to store your stuff in" }
    ```

=== "QS Inventory"

    ```lua
    ["furniture_stash_box"] = { ["name"] = "furniture_stash_box", ["label"] = "Furniture Stash Box", ["weight"] = 0, ["type"] = "item", ["image"] = "furniture_stash_box.png", ["unique"] = true, ["useable"] = true, ["shouldClose"] = true, ["combinable"] = nil, ["description"] = "A stash box to store your stuff in" }
    ```

## :material-database: SQL

Run the SQL file from the `sql/` folder.

## :material-plus-box: Adding new stash types

1. Create a new item in your inventory using the structure above. For ox_inventory, make sure you copy the `server.export` line so it triggers stash placement.
2. Add a matching entry to `Config.FurnitureStash` in `config.lua` — set the item name, size, slot count, and price.

## :material-help-circle: FAQ

??? question "My furniture stash isn't working"
    `snipe-motel` must start **before** `snipe-furniturestash` in `server.cfg`. This system only works with snipe-motel.
