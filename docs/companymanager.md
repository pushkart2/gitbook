---
icon: material/office-building-cog
---

# Company Manager

A modern boss menu for player-owned companies — hire/fire, change grades, and manage company funds with a full transaction log. Bosses can also place their own menu locations on the map (admin-controlled).

---

!!! abstract "Setup at a glance"
    Configure framework + banking backend → set admin identifiers → start the resource → bosses use `/bossmenuadmin` to place menu locations → players interact via target / marker.

## :material-package-variant-closed: Dependencies

- `ox_lib`
- `oxmysql`
- QBCore or ESX
- A banking backend (see [Banking integration](#banking-integration))
- *Optional:* a target resource — `ox_target`, `qb-target`, `qtarget`, or `interact`. Without one, the script falls back to a marker prompt.

## :material-cog: Configuration

Open `shared/config.lua` and adjust to your server.

### Framework

Auto-detected from running resources (`qb-core` or `es_extended`). Override the events in `Config.FrameworkTriggers` only if you've renamed the framework folder or its standard events.

### Banking integration

The script doesn't manage company funds itself — it routes deposits/withdrawals to your existing banking system.

```lua
Config.Banking = "snipe-banking"
-- valid values:
--   "snipe-banking"
--   "qb-banking"
--   "Renewed-Banking"
--   "qb-management"
--   "esx_society"
```

!!! tip "Pairs cleanly with snipe-banking"
    For full transaction tracking inside the boss menu, use `snipe-banking`. Other backends work but may not log transactions back into this UI.

### Target / interaction

```lua
Config.UseTarget = true
Config.Target    = "ox_target" -- "ox_target" | "qb-target" | "qtarget" | "interact"
```

If `Config.UseTarget = false` (or no target resource is running), the menu opens via marker + `[E]` prompt. Tune the visuals in `Config.BossMenuLocations`:

```lua
Config.BossMenuLocations = {
    DrawDistance       = 12.0,
    InteractDistance   = 1.6,
    TargetRadius       = 1.0,
    TargetDistance     = 2.0,
    TargetDrawDistance = 5.5,
    Marker = {
        type  = 2,
        scale = { x = 0.28, y = 0.28, z = 0.28 },
        color = { r = 0, g = 200, b = 150, a = 180 },
    },
}
```

### Notifications

```lua
Config.Notify = "ox"
```

### Admin permissions

Identifiers in `Config.BossMenuAdmin.Identifiers` and QBCore permission groups in `Config.BossMenuAdmin.QbPermissions` can access the **admin placement mode**.

```lua
Config.BossMenuAdmin = {
    Identifiers = {
        -- "license:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    },
    QbPermissions = {
        "god",
        "admin",
    },
}
```

## :material-console: Commands

| Command | Who | Action |
|---|---|---|
| `/bossmenuadmin` | Admins only | Open the admin UI to add, move, teleport to, or delete boss menu locations for any job. |

## :material-map-marker-plus: Placing a boss menu location

!!! note "Admins only"
    Only identifiers listed in `Config.BossMenuAdmin` can use this flow.

1. Run `/bossmenuadmin`.
2. Pick a job, then **Add Location**.
3. Aim at the spot where the menu should appear and press `G` to confirm (or cancel with the on-screen prompt).
4. To relocate a saved spot, choose **Move** and aim at the new position. To remove it, choose **Delete**.

## :material-account-tie: Using the boss menu

Players whose job grade is marked as boss (or `isboss = true` in QBCore) see the menu at every saved location for their job.

The menu has two tabs:

=== ":material-account-group: Management"

    - **Hire** a nearby player to your job (pick a grade).
    - **Fire** an existing employee.
    - **Change grade** for any employee — you cannot promote anyone to your own grade or higher.

=== ":material-bank: Account"

    - View current company funds (from your configured banking backend).
    - **Deposit** from personal bank to company.
    - **Withdraw** from company to personal bank.
    - Browse the transaction history.

!!! warning "Hiring guardrails"
    - You cannot hire a player who already works for this company.
    - You cannot fire yourself.
    - You cannot fire or modify anyone with a peer or higher grade.

## :material-code-tags: Exports

### Client

```lua
exports["snipe-companymanager"]:OpenBossMenu()
```

Opens the boss menu for the player's current job. Useful if you want to bind it to a custom command, item, or UI button.
