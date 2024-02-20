# Moonshine Script


## Install Items

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
```

## Install The SQL

- Check the sql file in Instal Me SQL folder and run the sql file in your database.

## Configure the script

- Check the shared folder and configure the script as you want. There are comments near every line to help you understand what each line does.