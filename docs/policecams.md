---
icon: material/cctv
---

# Police Cams

Bodycams, dashcams, bait cars, and a police tablet for live camera viewing.

---

## :material-package-down: Installation

1. Drop the resource into `resources/`. Make sure it starts at the **end** of `server.cfg`.
2. Read the config and tune to your server's needs.
3. Item images are in `assets/` (only needed if you go item-based).

## :material-package-variant: Item-based

=== "QBCore"

    Add to `qb-core/shared/items.lua`:

    ```lua
    ["bodycam"]      = { ["name"] = "bodycam",      ["label"] = "Bodycam",       ["weight"] = 2000, ["type"] = "item", ["image"] = "bodycam.png",      ["unique"] = false, ["useable"] = true, ["shouldClose"] = true, ["combinable"] = nil, ["description"] = "Bodycam" },
    ["dashcam"]      = { ["name"] = "dashcam",      ["label"] = "Dashcam",       ["weight"] = 2000, ["type"] = "item", ["image"] = "dashcam.png",      ["unique"] = false, ["useable"] = true, ["shouldClose"] = true, ["combinable"] = nil, ["description"] = "Dashcam" },
    ["policetablet"] = { ["name"] = "policetablet", ["label"] = "Police Tablet", ["weight"] = 2000, ["type"] = "item", ["image"] = "policetablet.png", ["unique"] = false, ["useable"] = true, ["shouldClose"] = true, ["combinable"] = nil, ["description"] = "Police Tablet" },
    ["gpsmodule"]    = { ["name"] = "gpsmodule",    ["label"] = "GPS Module",    ["weight"] = 2000, ["type"] = "item", ["image"] = "gpsmodule.png",    ["unique"] = false, ["useable"] = true, ["shouldClose"] = true, ["combinable"] = nil, ["description"] = "GPS Module to setup Bait Cars" },
    ```

=== "ESX"

    ```sql
    INSERT INTO `items` (`name`, `label`, `weight`, `rare`, `can_remove`) VALUES ('bodycam',      'Bodycam',       1, 0, 1);
    INSERT INTO `items` (`name`, `label`, `weight`, `rare`, `can_remove`) VALUES ('dashcam',      'Dashcam',       1, 0, 1);
    INSERT INTO `items` (`name`, `label`, `weight`, `rare`, `can_remove`) VALUES ('policetablet', 'Police Tablet', 1, 0, 1);
    INSERT INTO `items` (`name`, `label`, `weight`, `rare`, `can_remove`) VALUES ('gpsmodule',    'GPS Module',    1, 0, 1);
    ```

## :material-console: Command-based

| Command | Action |
|---|---|
| `/bodycam` | Toggle bodycam on/off |
| `/dashcam` | Install dashcam |
| `/baitcar` | Set up a bait car |
| `/cams` | View all active cameras |
| `/removedashcam` | Remove dashcam |
| `/removebaitcar` | Remove bait car |
