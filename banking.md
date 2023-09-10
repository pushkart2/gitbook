## SQL

- Install the sql in `INSTALL ME` folder
- Put the `snipe-banking` resource in your resources folder and ensure it in server.cfg at the end
- If you use any of the following management system mentioned below, check the steps your need to take.
## Config Changes

- If you have renamed your qb-core folder, es_extended folder or any other events, make sure to change them at respective places. If you use the base qb-core or es_extended, you dont need to change anything in `Config.FrameworkTriggers`
- If you are using ESX, make sure to read comments near `Config.JobAccounts` before turning it true.
- `Config.DefaultIdentifier` is the identifier to whom the loans will be transferred when a player who gave out loans, deletes his character.
- Read through the comments for other config options properly before making changes.

## QB-Management Conversion (Only if you use qb-managament.) DO NOT MAKE THIS CHANGES IF YOU USE renewed-banking
- Look for the things on left in your scripts and replace it to the ones on right.
- This change is important if you use qb-management or your money will be added to management_funds instead of the my database.
- Restart the server. Make sure nobody connects the server.
- Run the command `convertqbmanagement` in your server console once the server is up.
- This will also move all the job money from bank_accounts_new into my database.
- Restart the server again.
- Make sure to do the following export changes!!

```lua
exports['qb-management']:GetAccount => exports['snipe-banking']:GetAccount
exports['qb-management']:AddMoney => exports['snipe-banking']:GetAccount
exports['qb-management']:RemoveMoney => exports['snipe-banking']:RemoveMoney
exports['qb-management']:GetGangAccount=> exports['snipe-banking']:GetGangAccount
exports['qb-management']:AddGangMoney=> exports['snipe-banking']:AddGangMoney
exports['qb-management']:RemoveGangMoney=> exports['snipe-banking']:RemoveGangMoney
```

## Renewed Banking
- Restart the server. Make sure nobody connects the server.
- Run the command `convertrenewed` in your server console once the server is up.
- This will move everyone's personal account money into their bank account and delete the personal account information.
- This will also move all the job money from bank_accounts_new into my database.
- Restart the server again.

## QBCore 

### Changes in qb-core

- Look for the following function in qb-core/server/player.lua and replace it with the code below. Check the lines that say `--added`.

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
## ESX

### Changes in es_extended

- Look for this function in es_extended/server/classes/player.lua and replace it with the code below. Check the lines that say `--added`.

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

### Changes in esx_multicharacter Scripts (Only if you use it)
- If you use a custom esx_multicharacter script, you will have to ask the author of the script to find the similar event/function in their script so you can add the export

- Look for the following code in esx_multicharacter and add the export below it. Check the lines that say `--added`.

```lua
local function DeleteCharacter(source, charid)
	local identifier = ('%s%s:%s'):format(PREFIX, charid, GetIdentifier(source))
    exports["snipe-banking"]:DeleteCharacter(identifier) --added
	local query = 'DELETE FROM %s WHERE %s = ?'
	local queries = {}
	local count = 0

	for table, column in pairs(DB_TABLES) do
		count = count +  1
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


## Dispatch Changes
### Moz Dispatch

```lua
local function FlaggedAccount()
    local alertInfo = functions.getAlertInfo()
    local alert = {
        information = 'Flagged Account Accessed',
        priority = 'low',
        department = 'police',
        duration = 10000,
        code = '10-13A',
        location = alertInfo.location,
        blip = {
            radius = 50.0,
            sprite = 1,
            color = 1,
            scale = 1.0,
            alpha = 128,
            shortRange = true,
            name = 'Flagged Account',
            fadeAfter = 10000,
        }
    }
    TriggerServerEvent('moz-dispatch:server:sendAlert', alert)
end
exports('FlaggedAccount', FlaggedAccount)
```

### PS Dispatch

1. Open sv_dispatchcodes.lua and add the department you want to add to the list for the alert you want them to receive
```lua
 ["flagged_account"] =  {displayCode = '10-13', description ="Flagged Account", radius = 0, recipientList = {'police'}, blipSprite = 110, blipColour = 1, blipScale = 1.5, blipLength = 2, sound = "Lose_1st", sound2 = "GTAO_FM_Events_Soundset", offset = "false", blipflash = "false"},
 ```

 2. Open cl_events.lua and add the department to the corresponding alert to the one you edited above

```lua
local function FlaggedAccount()
    local currentPos = GetEntityCoords(PlayerPedId())
    local locationInfo = getStreetandZone(currentPos)
    local gender = GetPedGender()
    local PlayerPed = PlayerPedId()
    TriggerServerEvent("dispatch:server:notify", {
        dispatchcodename = "flagged_account", -- has to match the codes in sv_dispatchcodes.lua so that it generates the right blip
        dispatchCode = "10-11",
        firstStreet = locationInfo,
        gender = gender,
        model = nil,
        plate = nil,
        priority = 2,
        firstColor = nil,
        automaticGunfire = false,
        origin = {
            x = currentPos.x,
            y = currentPos.y,
            z = currentPos.z
        },
        dispatchMessage = "Flagged Account Accessed",
        job = {"police"} -- has to match the recipientList in sv_dispatchcodes.lua
    })
end
```



## Developer Exports

| Server Exports | Description |
| :--- | --- |
| IsAccountFlaggedForPlayerId(playerId)  | Returns true if the player's account is flagged. Parameter required is `player source id` |
| IsAccountFlaggedForIdentifier(playerIdentifier) | Returns true if the player's account is flagged. Parameter required is `player Identifier` (citizenid for qbcore and charid for esx). |
| IsAccountFrozenForPlayerId(playerId) | Returns true if the player's account is flagged. Parameter required is `player source id`. |
| IsAccountFrozenForIdentifier(playerIdentifier) | Returns true if the account is flagged. Parameter required is `player Identifier` (citizenid for qbcore and charid for esx) |
| DeleteCharacter(citizenid) | Should be triggered from place where the character is deleted. Parameter required is `citizenid` |
| CreatePersonalTransactions(identifier, amount, memo, type, otherPlayerIdentifier) | Should be triggered from place where the transaction is made. Parameters required are `identifier` (citizenid for qbcore and charid for esx), `amount`, `memo`, `type` (deposit/withdraw/transfer), `otherPlayerIdentifier` (citizenid for qbcore and charid for esx, if type is transfer) |
| CreateJobTransactions(jobgang, amount, memo, type, identifier, otherPlayerIdentifier) | Should be triggered from place where the transaction is made. Parameters required are `jobgang` (job name or gang name), `amount`, `memo`, `type` (deposit/withdraw/transfer), `identifier` (citizenid for qbcore and charid for esx), `otherPlayerIdentifier` (citizenid for qbcore and charid for esx, if type is transfer) |

| Client Exports | Description |
| :--- | --- |
| IsAccountFlagged() | Returns true if the player's account is flagged. |
| IsAccountFrozen() | Returns true if the player's account is Frozen. |


## FAQ

1. **I have frozen a person's account but he is still able to buy things using his bank account?**
- For QBCore, money is removed using the following function `Player.Functions.RemoveMoney`, but most of the scripts in qbcore dont rely on that false to be returned, instead they check the bank balance and then just trigger the RemoveMoney function and irrespective of the return value, they continue with the purchase. If I add a snippet for checking if player's bank account is frozen then return false, it will start giving them stuff for free. So you can use the exports provided above to do checks if account is frozen.
- For ESX, I have added a check in getAccount function to return 0 if his account is frozen. But again, it might work at some places while it may not at some places.

2. **Why are the money in bank account for job different from the money in the society account (ONLY ESX)?**
- Banking system doesnt support society accounts. I store my account money seperately which can be accessed only through bank ui. The reason for that being, society accounts are handled through 3-4 different scripts and has a lot of dependencies in ESX. If I were to change the files, it would be a lot of headache and very difficult to maintain in the long run. You are free to turn off the bank job accounts through config for ESX. It is by default on for QBCore.

