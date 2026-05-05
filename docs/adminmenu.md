---
icon: material/shield-crown
---

# Admin Menu

A role-based admin panel with permissions, panels, duty system, and developer tools.

!!! abstract "Setup at a glance"
    Configure → Compatibility → Permissions → Inventory events → optional ESX bans → optional weathersync patch → optional appearance patch → enable Admin Duty → optional locales/keybinds/webhooks → optional exports.

---

## :material-cog: Step 1 — Config

Read the comments in [config](#) and only edit the necessary things.

## :material-puzzle: Step 2 — Compatibility

Compatibility for multiple paid scripts is included. Read the comments in `config_compatibilty.lua` and make the changes accordingly.

## :material-account-key: Step 3 — Permissions

In the `Config` folder check `permissions.lua` — there are options to set whether the **God**, **Admin**, or **Mod** roles can access all the panels.

- Every role here can access the admin menu.
- Only the **God** role can set the panels for other roles.
- There are unlimited custom roles.

!!! warning "Do not remove the God role"
    The `God` role has access to every command and is required for assigning panels to other roles. **Never remove this role.**

If you have a new role, add it here and give it any value you want. It will then be available in the settings tab for God roles to select panels for.

```lua
["new_role"] = "God",
["dev"]      = "Admin",
```

```lua
Config.GodRoles = {
    ["god"]   = "God",
    ["admin"] = "Admin",
    ["mod"]   = "Moderator",
    -- add more roles here
}
```

## :material-package-variant: Step 4 — Inventory

=== "QB Inventory / lj-inventory"

    Add the following events at the **end** of `inventory/client/main.lua`.

    !!! danger "Do not add at the beginning"
        These events must be at the end of the file or they will be overridden.

    ```lua
    RegisterNetEvent('inventory:client:SetCurrentTrunk', function(vehicle)
        CurrentVehicle = vehicle
    end)

    RegisterNetEvent('inventory:client:SetCurrentGlovebox', function(vehicle)
        CurrentGlovebox = vehicle
    end)
    ```

## :material-gavel: Step 5 — Bans

!!! note "ESX only"
    Skip this step if you are not using ESX.

Run the `bans.sql` file from the `sql/` folder.

## :material-weather-partly-cloudy: Step 6 — qb-weathersync

If you use `qb-weathersync`, replace the following function in `qb-weathersync/server/main.lua`:

```lua
local function isAllowedToChange(src)
    if src == 0
        or QBCore.Functions.HasPermission(src, "admin")
        or IsPlayerAceAllowed(src, 'command')
        or exports["snipe-menu"]:isAdmin(src) then
        return true
    end
    return false
end
```

## :material-account-tie: Step 7 — Illenium Appearance + ESX

!!! note "ESX + Illenium Appearance only"
    Only required if you use both ESX **and** illenium-appearance.

Add the block below to `illenium-appearance/client/framework/esx/compatibility.lua`:

```lua
RegisterNetEvent("snipe-menu:client:openAppearance", function()
    local config           = GetDefaultConfig()
    config.ped             = true
    config.headBlend       = true
    config.faceFeatures    = true
    config.headOverlays    = true
    config.components      = true
    config.props           = true
    config.tattoos         = true
    OpenShop(config, true, "all")
end)
```

## :material-shield-check: Step 8 — Admin Duty

Since version **3.5.0+**, admins must go on duty to use the admin menu.

- Configure: `config/config_new.lua`
- Permissions: `config/permissions.lua`
- Toggle duty: `/adminduty`
- Webhooks for on/off duty available in `server/open/sv_webhooks.lua`

!!! tip
    If a player can't open the admin menu, they probably forgot `/adminduty`.

## :material-translate: Step 9 — Optional

### Adding a UI locale

1. Create a new file in `html/locales/` (e.g. `sp.json` for Spanish).
2. Copy the contents of `en.json` into it and translate the **right side** values only.
3. In `html/config.json`, change `lang` to your file name (e.g. `sp`).

```json
"Revive": "Reanimar",
```

!!! warning "Don't restart yet"
    Type `refresh` in the server console first so the new files are loaded, then run `ensure snipe-menu`.

### Keybinds

- Review `cl_keybinds.lua` before going live, or change them in **GTA 5 Settings → Keybinds → FiveM → snipe-menu**.
- Mouse keys are recommended for the delete laser.

!!! note "Dev mode only"
    Keybinds only work when **dev mode** is on (terminal button above the settings — God roles only).

### Discord logging

Update the webhook in `sv_webhooks.lua`. Almost every command and target is logged.

### Miscellaneous

Some client/server scripts are unencrypted and editable. Support for other paid scripts isn't planned, but the logic needed to integrate is left open where possible.

## :material-code-tags: Step 10 — Optional exports

=== "Dev mode"

    ```lua
    -- client
    exports["snipe-menu"]:isDevMode()

    -- server (source = playerId)
    exports["snipe-menu"]:isDevMode(source)
    ```

=== "Admin perms"

    ```lua
    -- client
    exports["snipe-menu"]:isAdmin()

    -- server (source = playerId)
    exports["snipe-menu"]:isAdmin(source)
    ```

=== "Spectating"

    ```lua
    exports["snipe-menu"]:isInSpectating()
    ```

=== "Admin role name"

    ```lua
    -- server (source = playerId)
    exports["snipe-menu"]:GetAdminRoleName(source)
    ```

## :material-tools: Developer options

Create your own custom panels and add them to the menu — everything you need is in the `custom/` folder. See `custom_config.lua` for all options.

Available component types inside a panel:

| Type | Description |
|---|---|
| `string-input` | Text input |
| `number-input` | Numeric input |
| `checkbox` | Boolean checkbox |
| `regular-dropdown` | Dropdown from a list, e.g. `{"Option 1", "Option 2"}` |
| `searchable-dropdown` | Searchable dropdown from objects, e.g. `{{id = 1, name = "Option 1"}, {id = 2, name = "Option 2"}}` |

!!! tip
    Check the `customs/` folder for working examples of custom panels and their callbacks.
