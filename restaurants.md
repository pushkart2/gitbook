# Ox inventory items
- Add the following items to the inventory. You dont need image. (You will never spawn this item)
```lua
    ["default_restaurant_item"] = {
		label = "Default Restaurant Item",
		weight = 150,
		stack = true,
		close = true,
		consume = 0,
		degrade = 2880,
		decay = true,
		-- degrade = 3 * 24 * 60,
		-- description = "Default restaurant item",
		server = {
			export = "snipe-customrestaurants.useRestaurantItem"
		}
	},

	["default_restaurant_bundle"] = {
		label = "Default Restaurant Bundle",
		weight = 150,
		stack = true,
		close = true,
		consume = 0,
		degrade = 2880,
		decay = true,
		-- degrade = 3 * 24 * 60,
		-- description = "Default restaurant item",
		server = {
			export = "snipe-customrestaurants.useBundle"
		}
	},
```
# Working
- The createPrice in config is for how much the business has to pay to make a custom food item. Set it to nil or 0 if you dont want to charge anything.
- The crafting requires items in the business stash for that business. The ingredients are not taken from players inventory.

# Config

- Make sure to read every line in config properly and available options
- SQL's auto install so you dont have to worry about it at all
- Add your admins who can manage the businesses in config.lua

# Stations (READ THIS VERY CAREFYLLY)

- You can create as many stations you want.
- Create item is a default station that only bosses of the job will have access to. Do not delete this station ever.
- They will be able to create items in game at this station.
- Other stations are for you to edit/add.
- Each line in there has information about what it does. 
- Effects and Ingredients will be discussed in another category below. Read that properly

# Effects
- By default the system comes with about following effects. You can add as many as you want by making changes in `function DoEffects` in client/open/cl_effects.lua.
- You can select what effects does each station category will apply.
- For eg. category food will give you "hunger" while joint will give you stress decrease + joint effect.
- There is something called `allowPlayerToSelectEffects` (check joints). This allows player to choose from 1 of the 3 available effects to apply along with the default ones you provide.
Available options where you can change the value according to your needs
```lua
    {effect = "food", value = 50}, -- increases food by 50
    {effect = "drink", value = 50}, -- increases drink by 50
    {effect = "alcohol", value = 20}, -- alcohol effect last 20 seconds
    {effect = "health", value = 20} -- health increase by 20
    {effect = "armor", value = 20} -- armor incrase by 20
    {effect = "stress", value = 20} -- reduces stress by 20 (set it negative value if you want to increase stress) (only configured for QB/QBX)
    {effect = "stamina", value = 20} -- unlimited stamina for 20 seconds
    {effect = "joint", value = 20} -- joint effect for 20 seconds

    { effect = "medical_focus" , value = 20} -- visual effects (value is in seconds)
    { effect = "adrenaline_rush", value = 20} -- visual effects (value is in seconds)
    { effect = "painkiller", value = 20} -- visual effects (value is in seconds)
    { effect = "weed_chill", value = 20} -- visual effects (value is in seconds)
    { effect = "weed_relax", value = 20} -- visual effects (value is in seconds)
    { effect = "weed_couchlock", value = 20} -- visual effects (value is in seconds)
```

# Ingredients
- This is the most important section which you will have to do properly
- If you go to shared/stations.lua, at the top you will see ingredients section.
- Think of this as each entry is a type of food item. For eg. base, sauces, chocolates, tobacco etc.
- When setting up the station, you say for eg. `ingredientCategories = {1, 2, 3, 4,6},`
- What this will do is, when owner of business is creating a food item in the create item station, they will have to choose 1 item from category 1, 1 item from category 2 and so on.
- So they will choose 5 items to make that food item and each item will be from that category.
- So for eg. your bases will be rice, dough, bun, etc which will take care of a sushi restaurant, pizza restaurant and burgers. Pizza restaurant for eg. will select dough as their base.

# Animations
- Animations is pretty easy to setup. 
- Based on the emote menu you use, you can take the emote name from your emote menu and add it to animations.lua and put them in the category you want at the bottom.
- Owner of the business can check the animation before selecting the animation while creating the food item.

# Webhooks
- All the webhooks can be added in server/open/sv_webhooks.lua

# Commands
- If you want to find offset on the vehicles to add, use command `/foodtruckoffset` and get the offsets. It will copy the offsets to clipboard when you press G