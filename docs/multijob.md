---
icon: material/briefcase-account
---

# Multi Job

Let players hold multiple jobs at once and switch between them on the fly.

---

## :material-package-down: Installation

1. Drop the resource into `resources/`. Make sure it starts at the **end** of `server.cfg` so all dependencies load first.
2. SQL is auto-imported on server start. If it fails, import it manually.
3. Read the config and tune to your server's needs.

## :material-package-variant-closed: Dependencies

- `ox_lib`
- `oxmysql`
- QBCore / ESX

## :material-code-tags: Exports

```lua
-- source     : the player triggering the removal
-- identifier : citizenid (QBCore) or charid (ESX) of the target
-- job        : the job name to remove from the player
exports["snipe-multijob"]:RemoveJob(source, identifier, job)
```

## :material-source-branch: qb-management — fire employee patch

Replace the `qb-bossmenu:server:FireEmployee` event in `qb-management/server/sv_boss.lua` (the only addition is the `RemoveJob` export call near the bottom):

```lua
-- Fire Employee
RegisterNetEvent('qb-bossmenu:server:FireEmployee', function(target)
    local src      = source
    local Player   = QBCore.Functions.GetPlayer(src)
    local Employee = QBCore.Functions.GetPlayerByCitizenId(target)

    if not Player.PlayerData.job.isboss then
        ExploitBan(src, 'FireEmployee Exploiting')
        return
    end

    if Employee then
        if target ~= Player.PlayerData.citizenid then
            if Employee.PlayerData.job.grade.level > Player.PlayerData.job.grade.level then
                TriggerClientEvent('QBCore:Notify', src, "You cannot fire this citizen!", "error")
                return
            end
            if Employee.Functions.SetJob("unemployed", '0') then
                TriggerEvent("qb-log:server:CreateLog", "bossmenu", "Job Fire", "red",
                    Player.PlayerData.charinfo.firstname .. " " .. Player.PlayerData.charinfo.lastname ..
                    ' successfully fired ' ..
                    Employee.PlayerData.charinfo.firstname .. " " .. Employee.PlayerData.charinfo.lastname ..
                    " (" .. Player.PlayerData.job.name .. ")", false)

                TriggerClientEvent('QBCore:Notify', src, "Employee fired!", "success")
                TriggerClientEvent('QBCore:Notify', Employee.PlayerData.source, "You have been fired! Good luck.", "error")
            else
                TriggerClientEvent('QBCore:Notify', src, "Error..", "error")
            end
        else
            TriggerClientEvent('QBCore:Notify', src, "You can't fire yourself", "error")
        end
    else
        local player = MySQL.query.await('SELECT * FROM players WHERE citizenid = ? LIMIT 1', { target })
        if player[1] ~= nil then
            Employee = player[1]
            Employee.job = json.decode(Employee.job)
            if Employee.job.grade.level > Player.PlayerData.job.grade.level then
                TriggerClientEvent('QBCore:Notify', src, "You cannot fire this citizen!", "error")
                return
            end
            local job          = {}
            job.name           = "unemployed"
            job.label          = "Unemployed"
            job.payment        = QBCore.Shared.Jobs[job.name].grades['0'].payment or 500
            job.onduty         = true
            job.isboss         = false
            job.grade          = {}
            job.grade.name     = nil
            job.grade.level    = 0
            MySQL.update('UPDATE players SET job = ? WHERE citizenid = ?', { json.encode(job), target })
            TriggerClientEvent('QBCore:Notify', src, "Employee fired!", "success")
            TriggerEvent("qb-log:server:CreateLog", "bossmenu", "Job Fire", "red",
                Player.PlayerData.charinfo.firstname .. " " .. Player.PlayerData.charinfo.lastname ..
                ' successfully fired ' ..
                Employee.PlayerData.charinfo.firstname .. " " .. Employee.PlayerData.charinfo.lastname ..
                " (" .. Player.PlayerData.job.name .. ")", false)
        else
            TriggerClientEvent('QBCore:Notify', src, "Civilian not in city.", "error")
        end
    end

    exports["snipe-multijob"]:RemoveJob(src, target, Player.PlayerData.job.name) -- add this line
    TriggerClientEvent('qb-bossmenu:client:OpenMenu', src)
end)
```

## :material-source-branch: No qb-policejob / qb-ambulancejob

!!! note "Skip if you use qb-policejob or qb-ambulancejob"
    Those scripts already handle duty toggling — only replace this if you don't use them.

Replace the `ToggleDuty` function in `client/open/cl_framework.lua`:

```lua
-- QBCore only. ESX changes go in server-side sv_framework.lua
function ToggleDuty()
    TriggerServerEvent("QBCore:ToggleDuty") -- present in qb-core/server/main.lua
end
```
