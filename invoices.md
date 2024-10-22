# Icebox Script
- Buy here - 
- Support - https://discord.gg/AeCVP2F8h7

# Steps for installation

1. Drag and Drop the resource to your server. Make sure it starts at the end of the list so all the dependencies are loaded first.
2. SQL file is automatically executed on server start.
3. Go through the config file and check all things to suit your server's need.

# Developer Exports

```lua
---@identifier string The identifier of the player to invoice
---@senderIdentifier string The identifier of the player sending the invoice
---@amount number The amount to invoice the player
---@reason string The reason for the invoice
---@job string The job of the player sending the invoice
exports["snipe-invoices"]:CreateCustomInvoiceOfflinePlayer(identifier, senderIdentifier, amount, reason, job)
```

```lua
---@source number The source of the player receiving the invoice
---@senderIdentifier string The identifier of the player sending the invoice
---@amount number The amount to invoice the player
---@reason string The reason for the invoice
---@job string The job of the player sending the invoice
exports["snipe-invoices"]:CreateCustomInvoiceOnlinePlayer(source, senderIdentifier, amount, reason, job)
```