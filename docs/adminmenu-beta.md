---
icon: material/flask-empty-outline
---

# Admin Menu (Beta)

A role-based admin panel with permissions, panels, duty system, developer tools, and a full **Tickets / Reports** system (new in beta).

!!! warning "Beta build"
    This page documents the **beta** version of `snipe-menu`. It includes everything from the stable [Admin Menu](adminmenu.md) docs plus the new beta features highlighted with a 🧪 beta badge. The UI, config, and database schema may shift between updates. Please report feedback in the Discord `#snipe-menu-beta` thread.

!!! abstract "Setup at a glance"
    Configure → Compatibility → Permissions → Inventory events → optional ESX bans → optional weathersync patch → optional appearance patch → enable Admin Duty → optional door-spawning convar → **Reports / Tickets (beta)** → optional locales/keybinds/webhooks → optional exports.

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

## :material-door: Step 9 — Dynamic door spawning &nbsp;:material-flask-empty-outline:{ title="Beta" }

!!! note "Only required if admins will spawn doors in-game"

GTA's dynamic door creation is gated behind a server convar. Add the line below to `server.cfg` so the doors admins place from the menu actually register at runtime:

```cfg
setr game_enableDynamicDoorCreation "true"
```

## :material-message-text: Step 10 — Reports & Tickets &nbsp;:material-flask-empty-outline:{ title="Beta" }

The old report/reply flow has been replaced with a dedicated **Tickets** system. It runs on a standalone UI, with categories, quick-replies, screenshot attachments, participants, and post-close admin ratings.

### Enable

In `config/config.lua`:

```lua
Config.EnableReports      = true                -- master switch for the new tickets system
Config.ScreenshotResource = "screenshot-basic"  -- "screenshot-basic" or "screencapture"
Config.ImageSaving        = "fivemanage"        -- key into ImageAPI in sv_ticket_screenshot.lua
```

!!! tip "Image API key"
    Open `server/open/sv_ticket_screenshot.lua` and paste your API key under the provider you picked (`fivemanage` or `qbox_cdn`). Without a valid key, the screenshot-attach flow silently returns `nil` instead of a URL — the rest of the ticket still works.

    ```lua
    ImageAPI = {
        ["fivemanage"] = {
            url = "https://api.fivemanage.com/api/v3/file/base64",
            api = "YOUR_API_KEY",
        },
        ["qbox_cdn"] = {
            url = "https://api.qbox.re/v1/file",
            api = "YOUR_API_KEY",
        },
    }
    ```

### Commands

| Command | Who | Action |
|---|---|---|
| `/report` | Everyone | Open the Reports UI — players file a ticket, admins see the queue. |
| `/togglereports` | Admins | Mute/unmute new-ticket notifications for your session. |

### Database tables

All tables are **auto-created on start** — no manual SQL. Tables added by the rebuild:

| Table | Purpose |
|---|---|
| `snipe_ticket_categories` | God-managed categories (Bug / Player Report / Question / Other are seeded by default). |
| `snipe_quick_tickets` | God-managed quick-reply preset templates. |
| `snipe_closed_tickets` | Archive of closed tickets — UI fetches on demand. |
| `snipe_admin_ratings` | Reporter ratings of admins (1 per closed ticket, unique on `ticket_id`). |

Open tickets are kept **in memory** and reset on resource restart — only closed tickets persist.

### Ticket lifecycle

```
reporter creates  →  open
admin claims      →  claimed (admin can leave internal notes)
admin closes      →  closed (reporter is prompted to rate the admin)
```

!!! note "One open ticket per reporter"
    Each `citizenid` can have **one open ticket at a time**. Admins bypass this when filing on behalf of someone else.

If the claiming admin disconnects, the ticket is auto-unclaimed and returns to the queue. If the reporter disconnects, a system message is posted in the ticket thread for the admin.

### Categories (god-managed)

Four categories are seeded on first start:

| Key | Label | Color |
|---|---|---|
| `bug` | Bug | red |
| `player_report` | Player Report | amber |
| `question` | Question | cyan |
| `other` | Other | neutral |

**Gods** (highest role tier in `config/permissions.lua`) can add/edit/delete categories from the Reports UI. Valid colors: `red`, `amber`, `cyan`, `green`, `purple`, `neutral`.

### Quick ticket presets (god-managed)

Gods can save canned messages per category — admins pick them from a dropdown when replying. Reporters can also use category-aware presets when filing.

### Screenshot attachments

While composing a message, an admin or reporter can capture an in-game screenshot to attach:

| Key | Action |
|---|---|
| ++e++ | Capture the current frame and upload |
| ++x++ | Cancel capture |

The image is uploaded via the configured `Config.ImageSaving` API and the resulting URL is appended to the ticket message.

### Participants

Tickets aren't private — an admin can **add other players** as participants (e.g. witnesses, secondary admins). Participants see and can reply to the ticket while it's open.

### Admin ratings

After a ticket is closed:

1. The reporter is shown a modal: 1–5 stars + optional 500-char comment.
2. The rating is stored in `snipe_admin_ratings`, one row per ticket.
3. Admins (and gods) can browse aggregate stats and per-admin history.

!!! warning "Gods only can clear stats"
    Bulk-clearing rating statistics is gated behind the **God** role. Regular admins can only view.

### Beta FAQ

??? question "Can I keep the old report system running side-by-side?"
    No — the legacy flow is replaced. The migration shim in `sv_reports_customise.lua` is a no-op kept temporarily so any clients still on the pre-rebuild build don't error; it will be removed in a future cleanup.

??? question "Why don't open tickets survive a restart?"
    Open tickets are intentionally in-memory — restarts during ongoing tickets are rare and rebuilding them from disk added complexity without much benefit. Closed tickets and ratings persist.

??? question "Image upload returns nothing"
    Three things to check:

    - `Config.ScreenshotResource` resource is actually started (`screenshot-basic` or `screencapture`).
    - You set a real API key in `server/open/sv_ticket_screenshot.lua` for the provider listed in `Config.ImageSaving`.
    - The provider key matches an entry in the `ImageAPI` table which can be changed in server/open/sv_ticket_screenshot.lua.

??? question "How do I migrate existing categories/presets?"
    There's nothing to migrate — the old report system didn't have these concepts. The 4 defaults are seeded automatically, and gods can extend them from the UI.

## :material-translate: Step 11 — Optional

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

## :material-code-tags: Step 12 — Optional exports

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

=== "Open Reports UI &nbsp;:material-flask-empty-outline:{ title='Beta' }"

    ```lua
    -- client — opens the Reports UI for the current player
    -- (auto-detects reporter vs admin role)
    exports["snipe-menu"]:OpenReports()
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
