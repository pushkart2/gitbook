---
icon: material/map-marker-radius
---

# Blips

Custom map blips and tracker items for police and gang members.

---

## :material-cog: Step 1 — Config

- Configure the config based on your framework.
- Read every comment in the config file before changing values.

## :material-package-variant: Step 2 — Items

=== "Ox Inventory"

    !!! tip "Use only this section for ox_inventory"
        Don't also add the items to a database `items.lua` — ox_inventory reads from its own table.

    ```lua
    ["tracker"] = {
        label = "Tracker",
        weight = 500,
        stack = true,
        close = true,
        description = "Tracker that lets you show other officers",
        client = {
            image = "tracker.png",
            remove = function(total)
                pcall(function() return exports["snipe-blips"]:RemoveTrackerItem() end)
            end
        },
    },

    ["gangtracker"] = {
        label = "gangtracker",
        weight = 500,
        stack = true,
        close = true,
        description = "Tracker that lets you show other Gang Members",
        client = {
            image = "tracker.png",
            remove = function(total)
                pcall(function() return exports["snipe-blips"]:RemoveTrackerItem() end)
            end
        },
    },
    ```

=== "QBCore (qb / lj inventory)"

    Add the items to your `database/items.lua`:

    ```lua
    ["tracker"]     = { ["name"] = "tracker",     ["label"] = "Tracker",      ["weight"] = 500, ["type"] = "item", ["image"] = "tracker.png", ["unique"] = false, ["useable"] = true, ["shouldClose"] = true, ["combinable"] = nil, ["description"] = "Tracker that lets you show other officers" },
    ["gangtracker"] = { ["name"] = "gangtracker", ["label"] = "Gang Tracker", ["weight"] = 500, ["type"] = "item", ["image"] = "tracker.png", ["unique"] = false, ["useable"] = true, ["shouldClose"] = true, ["combinable"] = nil, ["description"] = "Tracker that lets you show other gang members" },
    ```

=== "ESX"

    ```sql
    INSERT INTO `items` (`name`, `label`, `weight`, `rare`, `can_remove`) VALUES ('tracker', 'Tracker', 1, 0, 1);
    INSERT INTO `items` (`name`, `label`, `weight`, `rare`, `can_remove`) VALUES ('gangtracker', 'Gang Tracker', 1, 0, 1);
    ```

## :material-export: Step 3 — Export for item drop

=== "QB / PS / LJ Inventory"

    Add the snippet below at the indicated line in your inventory's `server/main.lua`:

    | Inventory | Reference line |
    |---|---|
    | [ps-inventory](https://github.com/Project-Sloth/ps-inventory/blob/84469dc164db586a5ceda24eca11be5ed94fc38a/server/main.lua#L228) | line 228 |
    | [qb-inventory](https://github.com/qbcore-framework/qb-inventory/blob/main/server/main.lua#L227) | line 227 |
    | [lj-inventory](https://github.com/loljoshie/lj-inventory/blob/main/server/main.lua#L209) | line 209 |

    ```lua
    if item == "tracker" or item == "gangtracker" then -- rename if you used different item names
        TriggerClientEvent("snipe-blips:client:RemoveTrackerItem", source)
    end
    ```

=== "ESX Inventory"

    !!! success "Already configured"
        No changes required.
