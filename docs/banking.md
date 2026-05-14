---
icon: material/bank
---

# Banking

Full-featured banking with personal accounts, job/gang accounts, loans, transactions, and dispatch alerts. Works with both **QBCore** and **ESX**.

!!! abstract "Setup at a glance"
    Drop in resource → configure → enable job auto-detection → run conversion command for your previous management system → patch core/inventory files → optional dispatch hookup.

---

## :material-package-down: Basic installation

- Put the `snipe-banking` resource in your `resources/` folder and **ensure it at the end** of `server.cfg`.
- If you currently use one of the management systems below, follow that section's conversion steps.

## :material-cog: Config changes

- If you've renamed your `qb-core` or `es_extended` folder (or any standard events), update them in `Config.FrameworkTriggers`. Vanilla folders need no changes.
- ESX users: read the comments around `Config.JobAccounts` carefully before turning it on.
- `Config.DefaultIdentifier` is the identifier loans get transferred to when the lender deletes their character.
- Read every comment in the config before changing values.

## :material-briefcase-account: Job accounts

`snipe-banking` auto-detects jobs on every server restart and creates a database entry for each — no manual setup required.

=== "QBCore"

    Mark any job/gang grade as a boss in `jobs.lua`:

    ```lua
    grades = {
        ['0'] = { name = 'Boss', isboss = true, payment = 0 }
    }
    ```

=== "ESX"

    Set one of the grade names to `boss` and it will be auto-detected.

## :material-swap-horizontal: Migrating from another management system

!!! danger "Pick exactly one"
    Run only the conversion that matches your **current** management system. Mixing them will corrupt account balances.

=== "qb-management"

    1. Restart the server. Make sure no one is connected.
    2. Run `convertqbmanagement` in the server console.
    3. This moves all job money from `management_funds` into the snipe-banking database.
    4. Restart the server again.
    5. In `qb-management/fxmanifest.lua`, comment out the server exports:

        ```lua
        -- server_exports {
        --     'AddMoney',
        --     'AddGangMoney',
        --     'RemoveMoney',
        --     'RemoveGangMoney',
        --     'GetAccount',
        --     'GetGangAccount',
        -- }
        ```

    6. Comment out the deposit/withdraw blocks in `qb-management/client/cl_boss.lua` and `cl_gang.lua` (reference screenshots: [boss](https://ibb.co/r0BJs1Z), [gang](https://ibb.co/7pJNyq5)).
    7. Jobs will now use the bank instead of `management_funds`.

=== "Renewed Banking"

    1. Restart with no players connected.
    2. Run `convertrenewed` in the server console.
    3. This moves personal account balances into bank accounts and clears the personal account table, plus migrates job money from `bank_accounts_new`.
    4. Restart the server.

=== "QB Banking"

    1. Restart with no players connected.
    2. Run `convertqbbanking` in the server console.
    3. Personal balances → bank, job money → snipe-banking tables.
    4. Restart the server.

## :material-source-branch: Framework patches

=== "QBCore"

    In `qb-core/server/player.lua`, replace `QBCore.Player.DeleteCharacter` with the version below. The `--added` line is the only new line:

    ```lua
    function QBCore.Player.DeleteCharacter(source, citizenid)
        exports["snipe-banking"]:DeleteCharacter(citizenid) --added
        local license = QBCore.Functions.GetIdentifier(source, 'license')
        local result = MySQL.scalar.await('SELECT license FROM players where citizenid = ?', { citizenid })
        if license == result then
            local query = "DELETE FROM %s WHERE citizenid = ?"
            local tableCount = #playertables
            local queries = table.create(tableCount, 0)

            for i = 1, tableCount do
                local v = playertables[i]
                queries[i] = {query = query:format(v.table), values = { citizenid }}
            end

            MySQL.transaction(queries, function(result2)
                if result2 then
                    TriggerEvent('qb-log:server:CreateLog', 'joinleave', 'Character Deleted', 'red', '**' .. GetPlayerName(source) .. '** ' .. license .. ' deleted **' .. citizenid .. '**..')
                end
            end)
        else
            DropPlayer(source, Lang:t("info.exploit_dropped"))
            TriggerEvent('qb-log:server:CreateLog', 'anticheat', 'Anti-Cheat', 'white', GetPlayerName(source) .. ' Has Been Dropped For Character Deletion Exploit', true)
        end
    end
    ```

=== "ESX"

    **es_extended** — replace `self.getAccount` in `es_extended/server/classes/player.lua`:

    ```lua
    function self.getAccount(account)
        if account == "bank" and exports["snipe-banking"]:IsAccountFrozenForIdentifier(self.identifier) then -- add this line
            return {name = "bank", money = 0}
        end
        for k,v in ipairs(self.accounts) do
            if v.name == account then
                return v
            end
        end
    end
    ```

    **esx_addonaccounts** — replace the contents of `esx_addonaccount/server/classes/addonaccount.lua`:

    ```lua
    local function TrimString(name)
        return name:sub(9) -- removes "society_" prefix
    end

    function CreateAddonAccount(name, owner, money)
        local self = {}

        self.name  = name
        self.job   = TrimString(name)
        self.owner = owner
        self.money = exports["snipe-banking"]:GetAccountBalance(self.job)

        function self.addMoney(m)
            self.money = self.money + m
            self.save()
            exports["snipe-banking"]:AddMoneyToAccount(self.job, m)
        end

        function self.removeMoney(m)
            self.money = self.money - m
            self.save()
            exports["snipe-banking"]:RemoveMoneyFromAccount(self.job, m)
        end

        function self.setMoney(m)
            self.money = m
            self.save()
            exports["snipe-banking"]:SetAccountMoney(self.job, m)
        end

        function self.save()
            TriggerClientEvent('esx_addonaccount:setMoney', -1, self.job, self.money)
        end

        return self
    end
    ```

    Then in `esx_addonaccounts/server/main.lua`, replace the entire `onResourceStart` event with:

    ```lua
    AddEventHandler('onResourceStart', function(resourceName)
        if resourceName == GetCurrentResourceName() then
            local accounts = MySQL.query.await('SELECT * FROM addon_account LEFT JOIN addon_account_data ON addon_account.name = addon_account_data.account_name UNION SELECT * FROM addon_account RIGHT JOIN addon_account_data ON addon_account.name = addon_account_data.account_name')

            local newAccounts = {}
            for i = 1, #accounts do
                local account = accounts[i]
                if account.shared == 0 then
                    if not Accounts[account.name] then
                        AccountsIndex[#AccountsIndex + 1] = account.name
                        Accounts[account.name] = {}
                    end
                    Accounts[account.name][#Accounts[account.name] + 1] = CreateAddonAccount(account.name, account.owner, account.money)
                end
            end

            if next(newAccounts) then
                MySQL.prepare('INSERT INTO addon_account_data (account_name, money) VALUES (?, ?)', newAccounts)
                for i = 1, #newAccounts do
                    local newAccount = newAccounts[i]
                    SharedAccounts[newAccount[1]] = CreateAddonAccount(newAccount[1], nil, 0)
                end
            end
        end
    end)

    CreateThread(function()
        while not GetResourceState('snipe-banking') == 'started' do
            Wait(100)
        end
        Wait(5000)
        local accounts = exports["snipe-banking"]:GetJobGangAccounts()
        for k, v in pairs(accounts) do
            local accountName = "society_"..k
            SharedAccounts[accountName] = CreateAddonAccount(accountName, nil, v)
        end
    end)
    ```

    Reference screenshot: [how it should look](https://cdn.discordapp.com/attachments/739152437645934632/1158184017661800541/image.png?ex=651b526b&is=651a00eb&hm=64fd2b735f5810cacca24771e28499f8a18e5f80f182acc9379ff4a72b1d5069&)

    !!! note "esx_multicharacter (custom forks)"
        If you use a custom multicharacter script, ask its author for the equivalent character-delete event.

    Add the export below the `DeleteCharacter` function:

    ```lua
    local function DeleteCharacter(source, charid)
        local identifier = ('%s%s:%s'):format(PREFIX, charid, GetIdentifier(source))
        exports["snipe-banking"]:DeleteCharacter(identifier) --added
        local query = 'DELETE FROM %s WHERE %s = ?'
        local queries = {}
        local count = 0

        for table, column in pairs(DB_TABLES) do
            count = count + 1
            queries[count] = {query = query:format(table, column), values = {identifier}}
        end

        MySQL.transaction(queries, function(result)
            if result then
                print(('[^2INFO^7] Player ^5%s %s^7 has deleted a character ^5(%s)^7'):format(GetPlayerName(source), source, identifier))
                Wait(50)
                SetupCharacters(source)
            else
                error('\n^1Transaction failed while trying to delete '..identifier..'^0')
            end
        end)
    end
    ```

    **Conversion command** — restart with no players connected, then run `convertesxsociety` to migrate `addon_account_data` into snipe-banking tables. Restart again.

## :material-bell-alert: Dispatch integration

=== "Moz Dispatch"

    ```lua
    local function FlaggedAccount()
        local alertInfo = functions.getAlertInfo()
        local alert = {
            information = 'Flagged Account Accessed',
            priority    = 'low',
            department  = 'police',
            duration    = 10000,
            code        = '10-13A',
            location    = alertInfo.location,
            blip = {
                radius     = 50.0,
                sprite     = 1,
                color      = 1,
                scale      = 1.0,
                alpha      = 128,
                shortRange = true,
                name       = 'Flagged Account',
                fadeAfter  = 10000,
            }
        }
        TriggerServerEvent('moz-dispatch:server:sendAlert', alert)
    end
    exports('FlaggedAccount', FlaggedAccount)
    ```

=== "PS Dispatch"

    !!! tip "ps-dispatch v2"
        No changes required if you're on v2.

    1. In `sv_dispatchcodes.lua`, add the alert and recipient list:

        ```lua
        ["flagged_account"] = {
            displayCode  = '10-13',
            description  = "Flagged Account",
            radius       = 0,
            recipientList = {'police'},
            blipSprite   = 110,
            blipColour   = 1,
            blipScale    = 1.5,
            blipLength   = 2,
            sound        = "Lose_1st",
            sound2       = "GTAO_FM_Events_Soundset",
            offset       = "false",
            blipflash    = "false"
        },
        ```

    2. In `cl_events.lua`, add the corresponding alert function:

        ```lua
        local function FlaggedAccount()
            local currentPos    = GetEntityCoords(PlayerPedId())
            local locationInfo  = getStreetandZone(currentPos)
            local gender        = GetPedGender()

            TriggerServerEvent("dispatch:server:notify", {
                dispatchcodename  = "flagged_account",
                dispatchCode      = "10-11",
                firstStreet       = locationInfo,
                gender            = gender,
                model             = nil,
                plate             = nil,
                priority          = 2,
                firstColor        = nil,
                automaticGunfire  = false,
                origin            = { x = currentPos.x, y = currentPos.y, z = currentPos.z },
                dispatchMessage   = "Flagged Account Accessed",
                job               = {"police"}
            })
        end

        exports("FlaggedAccount", FlaggedAccount)
        ```

## :material-code-tags: Developer exports

### Server

| Export | Description |
|---|---|
| `GetAccountBalance(jobOrGang)` | Returns the account balance. |
| `AddMoneyToAccount(jobOrGang, amount)` | Adds money. **Does not register a transaction** — use the transaction exports for that. |
| `RemoveMoneyFromAccount(jobOrGang, amount)` | Removes money. **Does not register a transaction.** |
| `IsAccountFlaggedForPlayerId(playerId)` | True if the player's account is flagged. |
| `IsAccountFlaggedForIdentifier(identifier)` | Same as above, by identifier (citizenid for QBCore, charid for ESX). |
| `IsAccountFrozenForPlayerId(playerId)` | True if the player's account is frozen. |
| `IsAccountFrozenForIdentifier(identifier)` | Same as above, by identifier. |
| `DeleteCharacter(citizenid)` | Call from wherever character deletion happens. |
| `CreatePersonalTransactions(identifier, amount, memo, type, otherPlayerIdentifier, isJob)` | Records a personal transaction. `type` is `deposit`/`withdraw`/`transfer`. Set `isJob = true` if the recipient is a job/gang. |
| `CreateJobTransactions(jobgang, amount, memo, type, identifier, otherPlayerIdentifier, isJob)` | Records a job transaction. |
| `CreateJobGangAccount(name, label, amount, isJob)` | Creates a new job/gang account. `isJob = true` for jobs, `false` for gangs. |

### Client

| Export | Description |
|---|---|
| `IsAccountFlagged()` | True if the player's account is flagged. |
| `IsAccountFrozen()` | True if the player's account is frozen. |
| `DoProgress(boolean)` | `true` shows the progress bar then opens the bank UI; `false` shows it then opens the ATM UI. |
| `OpenBank()` | Opens the bank UI without a progress bar. |
| `OpenATM()` | Opens the ATM UI without a progress bar. |

## :material-palette: Theming

The bank UI reads its colors from `shared/themes.lua`. Edit any value, restart the resource (or rebuild the UI), and the colors apply across every tab — header, dial, tabs, transactions, modals, graph.

```lua
Theme = {
    accent          = "#00C896",   -- titles, active tab, primary CTAs, rails
    secondaryAccent = "#3DD9B3",   -- user chip, frozen status, hover accents
    positive        = "#00C896",   -- success / positive amounts / deposit row
    negative        = "#EF4444",   -- danger / withdraw / errors
    info            = "#38BDF8",   -- transfer button + info chips
    highlight       = "#F59E0B",   -- export button + amber log entries
    background      = "#050505",   -- page background
    panel           = "#101214",   -- card / panel surface
    text            = "#F5F7FA",   -- primary text
    mutedText       = "#7B8496"    -- muted / secondary text
}
```

!!! tip "Each entry is a single hex"
    You only need a single hex per slot. The UI derives every alpha variant (06%, 12%, 22%, 30%, 55%, etc.) for borders, glows, and surfaces automatically.

!!! abstract "How it works"
    On UI mount, the Svelte app calls a `getTheme` NUI callback. The client returns the `Theme` table from `shared/themes.lua`. The UI converts each hex to an RGB triple and writes it into the matching CSS variable (`--accent-rgb`, `--secondary-rgb`, etc.). All theme-aware tokens (`--yellow-1`, `--blue-1`, `--green-1`, `--card-bg`, ...) are derived from those triples, so a single update cascades everywhere.

!!! example "Preset palettes"
    Swap the table out wholesale for a different feel. A few starters:

    === "Royal violet"

        ```lua
        Theme = {
            accent = "#A78BFA", 
            secondaryAccent = "#C4B5FD",
            positive = "#10B981", 
            negative = "#FB7185",
            info = "#60A5FA", 
            highlight = "#FDE68A",
            background = "#0A0612", 
            panel = "#15091F",
            text = "#F8FAFC", 
            mutedText = "#94A3B8"
        }
        ```

    === "Warm bronze"

        ```lua
        Theme = {
            accent = "#F59E0B", 
            secondaryAccent = "#FDE68A",
            positive = "#10B981", 
            negative = "#EF4444",
            info = "#38BDF8", 
            highlight = "#FBBF24",
            background = "#0B0907", 
            panel = "#160E08",
            text = "#FDF6E3", 
            mutedText = "#A8A29E"
        }
        ```

    === "Cyber cyan"

        ```lua
        Theme = {
            accent = "#22D3EE", 
            secondaryAccent = "#67E8F9",
            positive = "#10B981", 
            negative = "#F87171",
            info = "#60A5FA", 
            highlight = "#FACC15",
            background = "#020617", 
            panel = "#0F172A",
            text = "#F1F5F9", 
            mutedText = "#94A3B8"
        }
        ```

## :material-credit-card-check: Credit score

A loan-derived credit score (FICO-style, **300–850**) per player. Built into the bank UI as the **CREDIT** tab with a score dial, 8-week history graph, breakdown, and audit log. Designed to be read and mutated by other resources (dealerships, courts, insurance, etc.) via exports.

!!! abstract "How the score is built"
    Every player starts at the configured base (**650** by default). Their current score is recomputed live as:

    `score = clamp(base + loanComponent + manualAdjustments, 300, 850)`

    - **loanComponent** is derived from existing loan data — paid-off loans, early closures, active loans in good standing, and missed installments.
    - **manualAdjustments** is the sum of every `AddCreditScore` / `RemoveCreditScore` / `SetManualCreditScore` call from any resource. Every change is recorded in an audit log.
    - A weekly snapshot is taken on **every server start / resource restart** for any player overdue, powering the history graph.

### Configuration

The `Config.CreditScore` block in `shared/config.lua` controls range, snapshot cadence, and per-signal weights:

```lua
Config.CreditScore = {
    ShowUI = true,
    Base = 650,
    Min = 300,
    Max = 850,
    SnapshotIntervalDays = 7,
    Weights = {
        loanPaidOff           = 30,
        loanClosedEarly       = 15,
        missedInstallment     = -10,
        activeGoodStanding    =  5,
        loanDeclined          =  0
    }
}
```

!!! tip "Disabling the UI"
    Set `ShowUI = false` to hide the CREDIT tab while keeping the exports available for other resources.

### Server exports

All exports accept an `identifier` argument that is either a **citizenid / framework identifier string** or a **player source number** — resolved internally. Mutators return the new clamped total so callers don't need a follow-up `Get`. Every mutation is written to `banking_credit_score_log` with the calling resource recorded (taken from `GetInvokingResource()` if you don't pass one explicitly).

| Export | Description |
|---|---|
| `GetCreditScore(identifier)` | Returns the current clamped total score. Session-cached for 60s. |
| `GetCreditScoreBreakdown(identifier)` | Returns `{ base, fromLoans, manual, total, signals = { paidOff, closedEarly, activeGoodStanding, missedInstallments, declined } }`. Always fresh. |
| `AddCreditScore(identifier, amount, reason, sourceResource?)` | Increases the player's manual adjustment by `amount` (positive). Returns the new total. |
| `RemoveCreditScore(identifier, amount, reason, sourceResource?)` | Decreases the player's manual adjustment by `amount` (positive). Returns the new total. |
| `SetManualCreditScore(identifier, targetTotal, reason, sourceResource?)` | Overrides `manual_adjustment` so that the **final clamped total** matches `targetTotal`. Returns the new total. |
| `GetCreditScoreHistory(identifier, limit?)` | Returns recent audit-log rows: `{ delta, reason, source_resource, created_at }`. Default limit `25`. |
| `GetCreditScoreSnapshots(identifier, weeks?)` | Returns weekly snapshots for the history graph: `{ score, snapshot_at, delta }`. Default `8` weeks. |
| `InvalidateCreditScore(identifier)` | Clears the 60s session cache for an identifier. Optional — caches refresh automatically. |

### Usage example

```lua
local citizenid = QBCore.Functions.GetPlayer(source).PlayerData.citizenid

local score = exports['snipe-banking']:GetCreditScore(citizenid)
if score < 600 then
    TriggerClientEvent('ox_lib:notify', source, { type = 'error', description = 'Your credit score is too low for this purchase.' })
    return
end

local newTotal = exports['snipe-banking']:AddCreditScore(citizenid, 25, 'Completed contract', 'my-resource')
print(('Player credit is now %d'):format(newTotal))
```

!!! warning "Snapshots only run on server restart"
    The history graph fills as the resource restarts. For typical FiveM ops (daily/weekly restarts) snapshots accumulate naturally. If your server stays up longer than `SnapshotIntervalDays`, no new snapshot is taken until the next restart.

## :material-help-circle: FAQ

??? question "I froze a player's account but they can still buy things"
    **QBCore**: most scripts call `Player.Functions.RemoveMoney` and ignore its return value — they check the bank balance themselves and proceed regardless. Adding a frozen-check inside `RemoveMoney` would let them buy items for free. Use `IsAccountFrozenFor*` exports above to gate purchases yourself.

    **ESX**: `getAccount` returns 0 if frozen, but some scripts read balances differently and may slip through.

??? question "Job money deposited via management menu doesn't show in the bank (QBCore)"
    You skipped the [qb-management migration](#migrating-from-another-management-system). Jobs must use the bank UI, not the management deposit/withdraw flow.
