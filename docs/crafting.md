---
icon: material/hammer-wrench
---

# Crafting

Recipe-based crafting with blueprints, skill progression, and admin tools.

---

## :material-package-variant-closed: Dependencies

- `ox_inventory`
- QBCore / QBX / ESX

## :material-console: Commands

| Command | Purpose |
|---|---|
| `/crafting:admin` | Open admin crafting options |

## :material-package-down: Installation

- Read every comment in `shared/` — each line is documented.
- Set up your webhooks in `server/open/sv_webhooks.lua`.
- **Start the script last** in `server.cfg` so all dependencies load first.
- Database tables are created automatically — no manual SQL.
- All UI locales live in `html/locales/`.

Install the blueprint item (no image needed — blueprint images are configured per-recipe in `shared/recipes.lua`):

```lua
["crafting_blueprints"] = {
    label = "Blueprints",
    weight = 10,
    stack = false,
},
```

## :material-table-chair: Crafting tables (snipe-motel only)

!!! note "Only required if you use snipe-motel"
    Skip this section if you don't have snipe-motel.

- All images are in `snipe-crafting/images/`.
- **Start order**: snipe-crafting must come **after** snipe-motel (and after framework / ox_lib / target). Best practice — put it last in `server.cfg`.

```lua
["crafting_table_meth"] = {
    label = "Crafting Table (Meth)",
    weight = 0,
    stack = true,
    close = true,
    consume = 0,
    description = "Crafting Table",
    server = { export = "snipe-crafting.placeCraftingTable" }
},

["crafting_table_saw"] = {
    label = "Crafting Table (Saw)",
    weight = 0,
    stack = true,
    close = true,
    consume = 0,
    description = "Crafting Table",
    server = { export = "snipe-crafting.placeCraftingTable" }
},

["crafting_table_tool"] = {
    label = "Crafting Table (Tool)",
    weight = 0,
    stack = true,
    close = true,
    consume = 0,
    description = "Crafting Table",
    server = { export = "snipe-crafting.placeCraftingTable" }
},

["crafting_table_weapon"] = {
    label = "Crafting Table (Weapon)",
    weight = 0,
    stack = true,
    close = true,
    consume = 0,
    description = "Crafting Table",
    server = { export = "snipe-crafting.placeCraftingTable" }
},

["crafting_table_bigtool"] = {
    label = "Crafting Table (Big Tool)",
    weight = 0,
    stack = true,
    close = true,
    consume = 0,
    description = "Crafting Table",
    server = { export = "snipe-crafting.placeCraftingTable" }
},
```

## :material-code-tags: Server exports

Spawn a blueprint for a specific item with a craft limit (the item must be configured in `recipes.lua`):

```lua
exports["snipe-crafting"]:SpawnSpecificBlueprint(source, item, limit)
```

Spawn N random blueprints based on rarity weights from `recipes.lua`:

```lua
exports["snipe-crafting"]:SpawnRandomBlueprints(source, howmany)
```

!!! tip "Make sure every blueprint type exists"
    `SpawnRandomBlueprints` may sometimes give nothing if some types have no entries in `recipes.lua`.

### Examples

```lua
-- spawn a pistol blueprint with a limit of 5 crafts
exports["snipe-crafting"]:SpawnSpecificBlueprint(source, "weapon_pistol", 5)

-- spawn 5 random blueprints
exports["snipe-crafting"]:SpawnRandomBlueprints(source, 5)
```
