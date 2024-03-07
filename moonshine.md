# Moonshine Script


## Install Items 

### OX

```lua
    ["moonshine_bottle"] = {
		label = "Moonshine Bottle",
		weight = 1000,
		stack = false,
		close = true,
		description = "A bottle of moonshine",
		buttons = {
			{
				label = 'Get Dropoff Location',
				action = function(slot)
					exports["snipe-moonshine"]:GetDropOffLocation(slot)
				end
			},
			{
				label = 'Drink',
				action = function(slot)
					exports["snipe-moonshine"]:DrinkMoonshine(slot)
				end
			},
			{
				label = 'Drop Off',
				action = function(slot)
					exports["snipe-moonshine"]:DropOff(slot)
				end
			}
		},
	},
    ["moonshine_barrel"] = {
		label = "Moonshine Barrel",
		weight = 10000,
		stack = true,
		close = true,
		description = "A barrel of moonshine",
	},

	["moonshine_crate"] = {
		label = "Moonshine Crate",
		weight = 10000,
		stack = true,
		close = true,
		description = "A crate of moonshine",
	},

    ["mold_corn"] = {
		label = "Moldy Corn",
		weight = 100,
		stack = true,
		close = false,
		description = "A bag of Moldy corn",
	},
    ["mold_apple"] = {
		label = "Moldy Apple",
		weight = 100,
		stack = true,
		close = false,
		description = "A bag of Moldy apple",
	},
    ["mold_bread"] = {
		label = "Moldy Bread",
		weight = 100,
		stack = true,
		close = false,
		description = "A bag of moldy bread",
	},
	 ["mold_barley"] = {
		label = "Moldy Barley",
		weight = 100,
		stack = true,
		close = false,
		description = "A bag of Moldy barley",
	},
```

### QB/QS

```lua
 ["moonshine_bottle"] = {name = "moonshine_bottle", label = "Moonshine Bottle", weight = 100, type = 'item', image = 'moonshine_bottle.png', unique = true, useable = true, shouldClose = true, combinable = nil, description = 'A Moonshine Bottle'},
["moonshine_barrel"] = {name = "moonshine_barrel", label = "Moonshine Barrel", weight = 1000, type = 'item', image = 'moonshine_barrel.png', unique = true, useable = true, shouldClose = true, combinable = nil, description = 'A Moonshine Barrel'},
["moonshine_crate"] = {name = "moonshine_crate", label = "Moonshine Crate", weight = 1000, type = 'item', image = 'moonshine_crate.png', unique = true, useable = false, shouldClose = true, combinable = nil, description = 'A Moonshine Crate'},
["mold_corn"] = {name = "mold_corn", label = "Mold Corn", weight = 1000, type = 'item', image = 'mold_corn.png', unique = false, useable = false, shouldClose = true, combinable = nil, description = 'A moldy corn'},
["mold_apple"] = {name = "mold_apple", label = "Mold Apple", weight = 1000, type = 'item', image = 'mold_apple.png', unique = false, useable = false, shouldClose = true, combinable = nil, description = 'A moldy apple'},
["mold_bread"] = {name = "mold_bread", label = "Mold Bread", weight = 1000, type = 'item', image = 'mold_bread.png', unique = false, useable = false, shouldClose = true, combinable = nil, description = 'A moldy Bread'},
["mold_barley"] = {name = "mold_barley", label = "Mold Barley", weight = 1000, type = 'item', image = 'mold_barley.png', unique = false, useable = false, shouldClose = true, combinable = nil, description = 'A moldy Barley'},
["water"] = {name = "water", label = "Water", weight = 1000, type = 'item', image = 'water.png', unique = false, useable = false, shouldClose = true, combinable = nil, description = 'A bottle of water'},
```

## Install The SQL

- Check the sql file in Instal Me SQL folder and run the sql file in your database.

## Configure the script

- Check the shared folder and configure the script as you want. There are comments near every line to help you understand what each line does.

## Optional Changes for metadata showing on UI ( This is only for qb inventory). This change is not really required for the script to work!!

### NO SUPPORT WILL BE PROVIDED FOR ALL THE VERSIONS OF QB/PS/LJ INVENTORY! I WILL BE POSTING THE SNIPPET FOR LATEST QB INVENTORY AND IT WILL BE YOUR TASK TO MAKE IT WORK WITH OTHER. DO NOT OPEN TICKET FOR THIS!!!!

- Look for the following function in qb-inventory/html/js/app.js (https://github.com/qbcore-framework/qb-inventory/blob/main/html/js/app.js#L336)

```js
function generateDescription(itemData)
```

- Make the following changes in the function
```js
case "labkey": // this line already exists
	return `<p>Lab: ${itemData.info.lab}</p>`;  // this line already exists
case "moonshine_bottle": // add this line
	return `<p><strong>Quality: </strong><span>${itemData.info.moonshine_quality_label}</span></p>`; // add this line
default: // this line already exists
```