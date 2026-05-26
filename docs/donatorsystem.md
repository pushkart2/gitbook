---
icon: material/gift
---

# Donator System

Tebex-integrated donation rewards: gems/coins, daily shop, packages, and exploit detection.

---

## :material-store: Step 1 — Tebex store setup

Add this command to every Tebex package — it tells the script which package was bought and who bought it. [Video walkthrough](https://youtu.be/KMkVnL0Ur5U).

```lua
package_bought {"transaction_id":"{transaction}", "package":"{packageName}", "buyer_name":"{purchaserName}", "buyer_email":"{email}"}
```

Then register every package in `shared/packages.lua` — read the comments for the format.

## :material-cog: Step 2 — Config

- Read every comment in the `shared/` folder before changing values.
- Set your keybinds **before** starting the script for the first time.

## :material-package-variant: Step 3 — qb-inventory patch (latest only)

Add the following at the end of `qb-inventory/server/functions.lua`:

```lua
function RegisterInventory(identifier, data)
    local items = {}
    if Inventories[identifier] then
        items = Inventories[identifier].items
    end
    if not identifier then return end
    Inventories[identifier] = {
        items     = items,
        isOpen    = false,
        label     = data and data.label     or identifier,
        maxweight = data and data.maxweight or Config.StashSize.maxweight,
        slots     = data and data.slots     or Config.StashSize.slots
    }
end

exports('RegisterInventory', RegisterInventory)
```

## :material-palette: Step 4 — Theme

The donator UI is fully themed from a single file: **`shared/themes.lua`**. Every colour token in the UI is derived from this table — change a value here and the entire UI follows.

```lua
Theme = {
    accent          = "#00C896",  -- titles, primary CTAs, active accents, rails
    secondaryAccent = "#3DD9B3",  -- secondary tags / badges, soft accents
    positive        = "#00C896",  -- success / positive states
    negative        = "#EF4444",  -- danger / delete / errors
    info            = "#38BDF8",  -- info chips, totals, blue accents
    highlight       = "#F59E0B",  -- gems / price / amber highlights
    background      = "#050505",  -- page background
    panel           = "#101214",  -- card / panel surface
    text            = "#F5F7FA",  -- primary text
    mutedText       = "#7B8496"   -- muted / secondary text
}
```

!!! tip "How it works"
    The UI calls a `getTheme` NUI callback on load, the hex values get converted to RGB triples, and `:root` CSS variables (`--accent-rgb`, `--panel-rgb`, etc.) are overridden at runtime. Every CSS token used across the UI is derived from those triples — no rebuild required.

### Example — purple palette

A commented-out alternate palette is shipped in `shared/themes.lua`. Uncomment it (and comment out the green one) to switch the entire UI to purple:

```lua
Theme = {
    accent          = "#A855F7",
    secondaryAccent = "#C4B5FD",
    positive        = "#22C55E",
    negative        = "#FB7185",
    info            = "#22D3EE",
    highlight       = "#FBBF24",
    background      = "#0B0712",
    panel           = "#181226",
    text            = "#F5F3FF",
    mutedText       = "#8B82A6"
}
```

!!! note "Colour format"
    Values must be **hex** (`#RRGGBB` or `#RGB`). Anything else is ignored and the CSS default kicks in.

## :material-clock-outline: Daily shop time zones

Use any of these strings as `Config.DailyShop.time_zone`.

??? abstract "Show the full list of supported time zones"

    ```js
    'Europe/Andorra',
    'Asia/Dubai',
    'Asia/Kabul',
    'Europe/Tirane',
    'Asia/Yerevan',
    'Antarctica/Casey',
    'Antarctica/Davis',
    'Antarctica/DumontDUrville',
    'Antarctica/Mawson',
    'Antarctica/Palmer',
    'Antarctica/Rothera',
    'Antarctica/Syowa',
    'Antarctica/Troll',
    'Antarctica/Vostok',
    'America/Argentina/Buenos_Aires',
    'America/Argentina/Cordoba',
    'America/Argentina/Salta',
    'America/Argentina/Jujuy',
    'America/Argentina/Tucuman',
    'America/Argentina/Catamarca',
    'America/Argentina/La_Rioja',
    'America/Argentina/San_Juan',
    'America/Argentina/Mendoza',
    'America/Argentina/San_Luis',
    'America/Argentina/Rio_Gallegos',
    'America/Argentina/Ushuaia',
    'Pacific/Pago_Pago',
    'Europe/Vienna',
    'Australia/Lord_Howe',
    'Antarctica/Macquarie',
    'Australia/Hobart',
    'Australia/Currie',
    'Australia/Melbourne',
    'Australia/Sydney',
    'Australia/Broken_Hill',
    'Australia/Brisbane',
    'Australia/Lindeman',
    'Australia/Adelaide',
    'Australia/Darwin',
    'Australia/Perth',
    'Australia/Eucla',
    'Asia/Baku',
    'America/Barbados',
    'Asia/Dhaka',
    'Europe/Brussels',
    'Europe/Sofia',
    'Atlantic/Bermuda',
    'Asia/Brunei',
    'America/La_Paz',
    'America/Noronha',
    'America/Belem',
    'America/Fortaleza',
    'America/Recife',
    'America/Araguaina',
    'America/Maceio',
    'America/Bahia',
    'America/Sao_Paulo',
    'America/Campo_Grande',
    'America/Cuiaba',
    'America/Santarem',
    'America/Porto_Velho',
    'America/Boa_Vista',
    'America/Manaus',
    'America/Eirunepe',
    'America/Rio_Branco',
    'America/Nassau',
    'Asia/Thimphu',
    'Europe/Minsk',
    'America/Belize',
    'America/St_Johns',
    'America/Halifax',
    'America/Glace_Bay',
    'America/Moncton',
    'America/Goose_Bay',
    'America/Blanc-Sablon',
    'America/Toronto',
    'America/Nipigon',
    'America/Thunder_Bay',
    'America/Iqaluit',
    'America/Pangnirtung',
    'America/Atikokan',
    'America/Winnipeg',
    'America/Rainy_River',
    'America/Resolute',
    'America/Rankin_Inlet',
    'America/Regina',
    'America/Swift_Current',
    'America/Edmonton',
    'America/Cambridge_Bay',
    'America/Yellowknife',
    'America/Inuvik',
    'America/Creston',
    'America/Dawson_Creek',
    'America/Fort_Nelson',
    'America/Vancouver',
    'America/Whitehorse',
    'America/Dawson',
    'Indian/Cocos',
    'Europe/Zurich',
    'Africa/Abidjan',
    'Pacific/Rarotonga',
    'America/Santiago',
    'America/Punta_Arenas',
    'Pacific/Easter',
    'Asia/Shanghai',
    'Asia/Urumqi',
    'America/Bogota',
    'America/Costa_Rica',
    'America/Havana',
    'Atlantic/Cape_Verde',
    'America/Curacao',
    'Indian/Christmas',
    'Asia/Nicosia',
    'Asia/Famagusta',
    'Europe/Prague',
    'Europe/Berlin',
    'Europe/Copenhagen',
    'America/Santo_Domingo',
    'Africa/Algiers',
    'America/Guayaquil',
    'Pacific/Galapagos',
    'Europe/Tallinn',
    'Africa/Cairo',
    'Africa/El_Aaiun',
    'Europe/Madrid',
    'Africa/Ceuta',
    'Atlantic/Canary',
    'Europe/Helsinki',
    'Pacific/Fiji',
    'Atlantic/Stanley',
    'Pacific/Chuuk',
    'Pacific/Pohnpei',
    'Pacific/Kosrae',
    'Atlantic/Faroe',
    'Europe/Paris',
    'Europe/London',
    'Asia/Tbilisi',
    'America/Cayenne',
    'Africa/Accra',
    'Europe/Gibraltar',
    'America/Godthab',
    'America/Danmarkshavn',
    'America/Scoresbysund',
    'America/Thule',
    'Europe/Athens',
    'Atlantic/South_Georgia',
    'America/Guatemala',
    'Pacific/Guam',
    'Africa/Bissau',
    'America/Guyana',
    'Asia/Hong_Kong',
    'America/Tegucigalpa',
    'America/Port-au-Prince',
    'Europe/Budapest',
    'Asia/Jakarta',
    'Asia/Pontianak',
    'Asia/Makassar',
    'Asia/Jayapura',
    'Europe/Dublin',
    'Asia/Jerusalem',
    'Asia/Kolkata',
    'Indian/Chagos',
    'Asia/Baghdad',
    'Asia/Tehran',
    'Atlantic/Reykjavik',
    'Europe/Rome',
    'America/Jamaica',
    'Asia/Amman',
    'Asia/Tokyo',
    'Africa/Nairobi',
    'Asia/Bishkek',
    'Pacific/Tarawa',
    'Pacific/Enderbury',
    'Pacific/Kiritimati',
    'Asia/Pyongyang',
    'Asia/Seoul',
    'Asia/Almaty',
    'Asia/Qyzylorda',
    'Asia/Qostanay',
    'Asia/Aqtobe',
    'Asia/Aqtau',
    'Asia/Atyrau',
    'Asia/Oral',
    'Asia/Beirut',
    'Asia/Colombo',
    'Africa/Monrovia',
    'Europe/Vilnius',
    'Europe/Luxembourg',
    'Europe/Riga',
    'Africa/Tripoli',
    'Africa/Casablanca',
    'Europe/Monaco',
    'Europe/Chisinau',
    'Pacific/Majuro',
    'Pacific/Kwajalein',
    'Asia/Yangon',
    'Asia/Ulaanbaatar',
    'Asia/Hovd',
    'Asia/Choibalsan',
    'Asia/Macau',
    'America/Martinique',
    'Europe/Malta',
    'Indian/Mauritius',
    'Indian/Maldives',
    'America/Mexico_City',
    'America/Cancun',
    'America/Merida',
    'America/Monterrey',
    'America/Matamoros',
    'America/Mazatlan',
    'America/Chihuahua',
    'America/Ojinaga',
    'America/Hermosillo',
    'America/Tijuana',
    'America/Bahia_Banderas',
    'Asia/Kuala_Lumpur',
    'Asia/Kuching',
    'Africa/Maputo',
    'Africa/Windhoek',
    'Pacific/Noumea',
    'Pacific/Norfolk',
    'Africa/Lagos',
    'America/Managua',
    'Europe/Amsterdam',
    'Europe/Oslo',
    'Asia/Kathmandu',
    'Pacific/Nauru',
    'Pacific/Niue',
    'Pacific/Auckland',
    'Pacific/Chatham',
    'America/Panama',
    'America/Lima',
    'Pacific/Tahiti',
    'Pacific/Marquesas',
    'Pacific/Gambier',
    'Pacific/Port_Moresby',
    'Pacific/Bougainville',
    'Asia/Manila',
    'Asia/Karachi',
    'Europe/Warsaw',
    'America/Miquelon',
    'Pacific/Pitcairn',
    'America/Puerto_Rico',
    'Asia/Gaza',
    'Asia/Hebron',
    'Europe/Lisbon',
    'Atlantic/Madeira',
    'Atlantic/Azores',
    'Pacific/Palau',
    'America/Asuncion',
    'Asia/Qatar',
    'Indian/Reunion',
    'Europe/Bucharest',
    'Europe/Belgrade',
    'Europe/Kaliningrad',
    'Europe/Moscow',
    'Europe/Simferopol',
    'Europe/Kirov',
    'Europe/Astrakhan',
    'Europe/Volgograd',
    'Europe/Saratov',
    'Europe/Ulyanovsk',
    'Europe/Samara',
    'Asia/Yekaterinburg',
    'Asia/Omsk',
    'Asia/Novosibirsk',
    'Asia/Barnaul',
    'Asia/Tomsk',
    'Asia/Novokuznetsk',
    'Asia/Krasnoyarsk',
    'Asia/Irkutsk',
    'Asia/Chita',
    'Asia/Yakutsk',
    'Asia/Khandyga',
    'Asia/Vladivostok',
    'Asia/Ust-Nera',
    'Asia/Magadan',
    'Asia/Sakhalin',
    'Asia/Srednekolymsk',
    'Asia/Kamchatka',
    'Asia/Anadyr',
    'Asia/Riyadh',
    'Pacific/Guadalcanal',
    'Indian/Mahe',
    'Africa/Khartoum',
    'Europe/Stockholm',
    'Asia/Singapore',
    'America/Paramaribo',
    'Africa/Juba',
    'Africa/Sao_Tome',
    'America/El_Salvador',
    'Asia/Damascus',
    'America/Grand_Turk',
    'Africa/Ndjamena',
    'Indian/Kerguelen',
    'Asia/Bangkok',
    'Asia/Dushanbe',
    'Pacific/Fakaofo',
    'Asia/Dili',
    'Asia/Ashgabat',
    'Africa/Tunis',
    'Pacific/Tongatapu',
    'Europe/Istanbul',
    'America/Port_of_Spain',
    'Pacific/Funafuti',
    'Asia/Taipei',
    'Europe/Kiev',
    'Europe/Uzhgorod',
    'Europe/Zaporozhye',
    'Pacific/Wake',
    'America/New_York',
    'America/Detroit',
    'America/Kentucky/Louisville',
    'America/Kentucky/Monticello',
    'America/Indiana/Indianapolis',
    'America/Indiana/Vincennes',
    'America/Indiana/Winamac',
    'America/Indiana/Marengo',
    'America/Indiana/Petersburg',
    'America/Indiana/Vevay',
    'America/Chicago',
    'America/Indiana/Tell_City',
    'America/Indiana/Knox',
    'America/Menominee',
    'America/North_Dakota/Center',
    'America/North_Dakota/New_Salem',
    'America/North_Dakota/Beulah',
    'America/Denver',
    'America/Boise',
    'America/Phoenix',
    'America/Los_Angeles',
    'America/Anchorage',
    'America/Juneau',
    'America/Sitka',
    'America/Metlakatla',
    'America/Yakutat',
    'America/Nome',
    'America/Adak',
    'Pacific/Honolulu',
    'America/Montevideo',
    'Asia/Samarkand',
    'Asia/Tashkent',
    'America/Caracas',
    'Asia/Ho_Chi_Minh',
    'Pacific/Efate',
    'Pacific/Wallis',
    'Pacific/Apia',
    'Africa/Johannesburg'
    ```

## :material-tools: Developer options

### Commands

| Command | Action |
|---|---|
| `/addgems <playerid> <amount>` | Add gems to a player |
| `/removegems <playerid> <amount>` | Remove gems from a player |

### Exports

```lua
exports["snipe-donatorsystem"]:AddCoins(playerid, amount)    -- add coins
exports["snipe-donatorsystem"]:RemoveCoins(playerid, amount) -- remove coins
exports["snipe-donatorsystem"]:GetCoins(playerid)            -- returns coin balance
```

## :material-shield-search: Exploit detection query

Latest versions log every coin redemption to a separate table. Run the query below periodically to find players who claimed more than they should have (for example via network lag-switch exploits). An empty result means everyone is clean.

```sql
SELECT
    c.transaction_id,
    r.name,
    r.identifier,
    c.total_codes_coins,
    r.total_redeemed_coins
FROM (
    SELECT transaction_id, SUM(coins) AS total_codes_coins
    FROM snipe_donator_codes
    WHERE redeemed = '1'
    GROUP BY transaction_id
) c
LEFT JOIN (
    SELECT transaction_id, name, identifier, SUM(coins) AS total_redeemed_coins
    FROM snipe_donator_redeemed
    GROUP BY transaction_id
) r ON c.transaction_id = r.transaction_id
WHERE r.total_redeemed_coins IS NOT NULL
  AND IFNULL(c.total_codes_coins, 0) != IFNULL(r.total_redeemed_coins, 0);
```
