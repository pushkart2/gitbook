---
icon: material/snowflake
---

# Icebox

Wearable jewelry and clothing — chains, watches, masks, hats — with infusable diamonds/rubies for visual buffs and shop-based purchasing.

[:fontawesome-solid-cart-shopping: Buy on Tebex](https://snipe.tebex.io/package/5781971){.md-button .md-button--primary}
[:fontawesome-brands-discord: Discord support](https://discord.gg/AeCVP2F8h7){.md-button}

---

!!! info "Update notice (Oct 10, 2024)"
    Two new parameters were added to every chain/watch:

    - **`itemPrice`** — price when sold at a buy location. Falls back to `Config.DefaultPrice` if omitted.
    - **`shopType`** — restricts the item to specific shop(s). Omit to make it available everywhere.

    With these, you can scope items to specific shops (e.g. a "mask shop" that only sells masks) and skip the crafting flow by selling chains/watches directly.

    Players can now wear **multiple chains and watches at once** as long as they belong to different `componentId` and `propId`. e.g. a chain on `componentId 7` and another on `componentId 9` can both be worn together.

    Your existing `chains.lua` / `watches.lua` files keep working — but the new features only apply if you add the parameters manually.

## :material-source-branch: Inventory patch (QBCore only)

In `inventory/server/main.lua`, find the function `local function RemoveItem(source, item, amount, slot)` and add the line below underneath the `if not Player then return false end` check:

```lua
if not Player then return false end
TriggerClientEvent("snipe-icebox:client:removeItem", source, item) -- add this line
```

## :material-package-variant: Items

!!! note "Don't double-add items"
    If any of these item names are already added by another snipe script, don't add them again.

=== "Ox Inventory"

    !!! tip "When adding new chains"
        Make sure you preserve the `server.export` field — that's what triggers chain logic on use.

    ```lua
    ["clothing_chain"] = {
        label   = "Clothing Chain",
        weight  = 1,
        stack   = false,
        close   = true,
        consume = 0,
        server  = { export = 'snipe-icebox.useChain' }
    },
    ["prop_chain"] = {
        label   = "Prop Chain",
        weight  = 1,
        stack   = false,
        close   = true,
        consume = 0,
        server  = { export = 'snipe-icebox.useChain' }
    },
    ["mask_pigface"] = {
        label   = "PigFace Mask",
        weight  = 1,
        stack   = false,
        close   = true,
        consume = 0,
        server  = { export = 'snipe-icebox.useChain' }
    },
    ["clothing_watch"] = {
        label   = "Clothing Watch",
        weight  = 1,
        stack   = false,
        close   = true,
        consume = 0,
        server  = { export = 'snipe-icebox.useWatch' }
    },

    ["diamond"]     = { label = "Diamond",     weight = 1, stack = true, close = true },
    ["ruby"]        = { label = "Ruby",        weight = 1, stack = true, close = true },
    ["buff_diamond"]= { label = "Buff Diamond", weight = 1, stack = true, close = true },
    ["buff_ruby"]   = { label = "Buff Ruby",    weight = 1, stack = true, close = true },
    ```

=== "QB Inventory"

    ```lua
    ["prop_chain"]      = { ["name"] = "prop_chain",      ["label"] = "Chain as Prop",      ["weight"] = 0, ["type"] = "item", ["image"] = "prop_chain.png",      ["unique"] = true,  ["useable"] = true,  ["shouldClose"] = true, ["combinable"] = nil, ["description"] = "Example Chain for Prop" },
    ["clothing_chain"]  = { ["name"] = "clothing_chain",  ["label"] = "Chain as Clothing",  ["weight"] = 0, ["type"] = "item", ["image"] = "clothing_chain.png",  ["unique"] = false, ["useable"] = true,  ["shouldClose"] = true, ["combinable"] = nil, ["description"] = "Example Chain for Clothing item" },
    ["mask_pigface"]    = { ["name"] = "mask_pigface",    ["label"] = "Pigface Mask",       ["weight"] = 0, ["type"] = "item", ["image"] = "mask_pigface.png",    ["unique"] = false, ["useable"] = true,  ["shouldClose"] = true, ["combinable"] = nil, ["description"] = "Example Chain for Mask Item" },
    ["clothing_watch"]  = { ["name"] = "clothing_watch",  ["label"] = "Watch as Clothing",  ["weight"] = 0, ["type"] = "item", ["image"] = "clothing_watch.png",  ["unique"] = false, ["useable"] = true,  ["shouldClose"] = true, ["combinable"] = nil, ["description"] = "Example Watch for Clothing item" },
    ["ruby"]            = { ["name"] = "ruby",            ["label"] = "Ruby",               ["weight"] = 0, ["type"] = "item", ["image"] = "ruby.png",            ["unique"] = false, ["useable"] = false, ["shouldClose"] = true, ["combinable"] = nil, ["description"] = "Ruby" },
    ["buff_diamond"]    = { ["name"] = "buff_diamond",    ["label"] = "Diamond(Buff)",      ["weight"] = 0, ["type"] = "item", ["image"] = "diamond.png",         ["unique"] = false, ["useable"] = false, ["shouldClose"] = true, ["combinable"] = nil, ["description"] = "Diamonds use to buff chains/watches" },
    ["buff_ruby"]       = { ["name"] = "buff_ruby",       ["label"] = "Ruby(Buff)",         ["weight"] = 0, ["type"] = "item", ["image"] = "ruby.png",            ["unique"] = false, ["useable"] = false, ["shouldClose"] = true, ["combinable"] = nil, ["description"] = "Ruby use to buff chains/watches" },
    ```

=== "QS Inventory"

    ```lua
    ['prop_chain']     = { ['name'] = 'prop_chain',     ['label'] = 'Chain as Prop',     ['weight'] = 500, ['type'] = 'item', ['image'] = 'prop_chain.png',     ['unique'] = true, ['useable'] = true, ['shouldClose'] = false, ['combinable'] = nil, ['description'] = 'none' },
    ['clothing_chain'] = { ['name'] = 'clothing_chain', ['label'] = 'Chain as Clothing', ['weight'] = 500, ['type'] = 'item', ['image'] = 'clothing_chain.png', ['unique'] = true, ['useable'] = true, ['shouldClose'] = false, ['combinable'] = nil, ['description'] = 'none' },
    ['clothing_watch'] = { ['name'] = 'clothing_watch', ['label'] = 'Watch as Clothing', ['weight'] = 500, ['type'] = 'item', ['image'] = 'clothing_watch.png', ['unique'] = true, ['useable'] = true, ['shouldClose'] = false, ['combinable'] = nil, ['description'] = 'none' },
    ['mask_pigface']   = { ['name'] = 'mask_pigface',   ['label'] = 'PigFace Mask',      ['weight'] = 500, ['type'] = 'item', ['image'] = 'mask_pigface.png',   ['unique'] = true, ['useable'] = true, ['shouldClose'] = false, ['combinable'] = nil, ['description'] = 'none' },
    ['diamond']        = { ['name'] = 'diamond',        ['label'] = 'Diamond',           ['weight'] = 500, ['type'] = 'item', ['image'] = 'diamond.png',        ['unique'] = true, ['useable'] = true, ['shouldClose'] = false, ['combinable'] = nil, ['description'] = 'none' },
    ['ruby']           = { ['name'] = 'ruby',           ['label'] = 'Ruby',              ['weight'] = 500, ['type'] = 'item', ['image'] = 'ruby.png',           ['unique'] = true, ['useable'] = true, ['shouldClose'] = false, ['combinable'] = nil, ['description'] = 'none' },
    ['buff_diamond']   = { ['name'] = 'buff_diamond',   ['label'] = 'Diamond(Buff)',     ['weight'] = 500, ['type'] = 'item', ['image'] = 'diamond.png',        ['unique'] = true, ['useable'] = true, ['shouldClose'] = false, ['combinable'] = nil, ['description'] = 'none' },
    ['buff_ruby']      = { ['name'] = 'buff_ruby',      ['label'] = 'Ruby(Buff)',        ['weight'] = 500, ['type'] = 'item', ['image'] = 'ruby.png',           ['unique'] = true, ['useable'] = true, ['shouldClose'] = false, ['combinable'] = nil, ['description'] = 'none' },
    ```

## :material-plus-box: Adding new chain/watch items

1. Create a new inventory item using `clothing_chain` / `clothing_watch` as a template.

    !!! danger "Inventory-specific syntax"
        Don't copy-paste an ox_inventory definition into qb/qs and vice versa. The format is completely different.

    Example for **ox_inventory**:

    ```lua
    ["test_chain"] = {
        label   = "Test Chain",
        weight  = 1,
        stack   = false,
        close   = true,
        consume = 0,
        server  = { export = 'snipe-icebox.useChain' }
    },
    ```

2. Configure the chain/watch in `snipe-icebox/shared/chains.lua` or `snipe-icebox/shared/watches.lua`:

    ```lua
    ["test_chain"] = {
        label       = "Test Chain",
        drawable    = true,                -- true = clothing, false = prop
        category    = "business",
        itemPrice   = 5000,                -- buy price (falls back to Config.DefaultPrice)
        componentId = 5,                   -- override component ID (default 7)
        clothing    = {
            ["mp_m_freemode_01"] = { drawable = 82, texture = 0 }, -- male ped
        },

        -- offsets enables infused-item particle effects; comment out to disable
        -- offsets = {
        --     ["diamond"] = vec3(0.0, 0.08, 0.34),
        --     ["ruby"]    = vec3(0.0, 0.14, 0.36),
        -- },

        craftItems = {
            ["diamond"] = { label = "Diamond", amount = 1 },
            ["ruby"]    = { label = "Ruby",    amount = 1 },
        }
    },
    ```

## :material-compare: Chains vs watches

| | Chains | Watches |
|---|---|---|
| Native | Component variation | Prop variation |
| Reference | [SetPedComponentVariation](https://docs.fivem.net/natives/?_0x262B14F48D29DE80) | [SetPedPropIndex](https://docs.fivem.net/natives/?_0x93376B65A266EB5F) |
| Use cases | Necklaces, masks (componentId 1) | Hats (componentId 0), watches |

So a mask goes in `chains.lua` (it's a component variation), and a hat goes in `watches.lua` (it's a prop variation).

## :material-tune: Optional integrations

!!! tip "Apartments / houses"
    Prop chains can cause weird visual artifacts in cars after entering apartments. Call the export below on house/apartment enter and exit:

    ```lua
    exports["snipe-icebox"]:RemoveChains()
    ```

## :material-bug: Known issues

??? question "I can't see the UI even though pressing E opens the context menu / safe"
    Set `Config.UseLibTextUI = false`. Read the comment on that config line for context.

??? question "Particle effects aren't showing"
    The offset for that effect probably isn't set in `Config.Chains`, or that effect doesn't apply to the chain. Test with the base effects shipped with the script first to confirm visibility before adding new infuse items.
