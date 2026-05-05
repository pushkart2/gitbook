---
icon: material/silverware-fork-knife
---

# Restaurants

Custom restaurant ordering, menu cards, station-based food crafting, effects, and food trucks.

---

## :material-package-variant: Step 1 — Items

Add the following items to **ox_inventory**. Images aren't required (these items aren't spawned directly).

```lua
["default_restaurant_item"] = {
    label   = "Default Restaurant Item",
    weight  = 150,
    stack   = true,
    close   = true,
    consume = 0,
    degrade = 2880,
    decay   = true,
    server  = { export = "snipe-customrestaurants.useRestaurantItem" }
},

["default_restaurant_bundle"] = {
    label   = "Default Restaurant Bundle",
    weight  = 150,
    stack   = true,
    close   = true,
    consume = 0,
    degrade = 2880,
    decay   = true,
    server  = { export = "snipe-customrestaurants.useBundle" }
},

["default_menu_card"] = {
    label   = "Default Menu Card",
    weight  = 150,
    stack   = true,
    close   = true,
    consume = 0,
    degrade = 10,    -- 10 minutes — auto-deletes from pocket so people don't stack them
    decay   = true,
    server  = { export = "snipe-customrestaurants.useMenuCard" }
},
```

## :material-cog: Step 2 — How it works

- `Config.createPrice` controls how much a business pays to create a custom food item. Set to `nil` or `0` to make creation free.
- Crafting **takes ingredients from the business stash**, not the player's inventory.

## :material-tools: Step 3 — Config

- Read every line in the config — every option is documented inline.
- SQL is auto-installed; nothing to import manually.
- Add the admin identifiers who can manage businesses in `config.lua`.

## :material-table-furniture: Step 4 — Stations

!!! warning "Read this carefully"
    Misconfigured stations are the #1 source of restaurant issues.

- You can create as many stations as you want.
- **`Create Item`** is a default station only the **boss** of a job can access. **Never delete this station** — it's how owners create new menu items in-game.
- All other stations are yours to add or modify.
- Every line in the station config is commented — read them all.
- See [Effects](#effects) and [Ingredients](#ingredients) below for the related concepts.

## :material-flash: Effects

The system ships with the effects below. Add more by editing the `DoEffects` function in `client/open/cl_effects.lua`. Each station category can apply different effects (e.g. food → hunger, joints → stress decrease + visuals).

!!! tip "Player-selectable effects"
    Some categories support `allowPlayerToSelectEffects` (see joints) — this lets the consumer pick 1 of 3 effects on top of the defaults.

```lua
{ effect = "food",     value = 50 }, -- +50 hunger
{ effect = "drink",    value = 50 }, -- +50 thirst
{ effect = "alcohol",  value = 20 }, -- alcohol effect for 20 seconds
{ effect = "health",   value = 20 }, -- +20 health
{ effect = "armor",    value = 20 }, -- +20 armor
{ effect = "stress",   value = 20 }, -- -20 stress (negative value increases stress; QB/QBX only)
{ effect = "stamina",  value = 20 }, -- unlimited stamina for 20s
{ effect = "joint",    value = 20 }, -- joint visual effect for 20s

{ effect = "medical_focus",   value = 20 }, -- visual effect (seconds)
{ effect = "adrenaline_rush", value = 20 }, -- visual effect (seconds)
{ effect = "painkiller",      value = 20 }, -- visual effect (seconds)
{ effect = "weed_chill",      value = 20 }, -- visual effect (seconds)
{ effect = "weed_relax",      value = 20 }, -- visual effect (seconds)
{ effect = "weed_couchlock",  value = 20 }, -- visual effect (seconds)
```

## :material-food-variant: Ingredients

This is the most important section to set up correctly.

- Open `shared/stations.lua` — at the top is the **ingredients** section.
- Each entry is a **type** of food item (e.g. base, sauces, chocolates, tobacco).
- When defining a station, you set `ingredientCategories = {1, 2, 3, 4, 6}` — that means the owner picks **one ingredient from each listed category** when creating a menu item.
- So with 5 categories, the owner picks 5 ingredients per item.

**Example design**: your "base" category has rice, dough, bun, etc. A pizza restaurant picks `dough` as its base; a sushi restaurant picks `rice`.

## :material-animation: Animations

- Animations setup is straightforward.
- Look up the emote name in your emote menu, then add it to `animations.lua` under the appropriate category at the bottom of the file.
- The business owner can preview the animation when picking one for a food item.

## :material-discord: Webhooks

All webhooks are configured in `server/open/sv_webhooks.lua`.

## :material-console: Commands

| Command | Action |
|---|---|
| `/foodtruckoffset` | Get vehicle offsets — press `G` while running it to copy the offset to clipboard |
