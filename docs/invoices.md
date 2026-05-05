---
icon: material/receipt-text
---

# Invoices

Send and pay invoices between players and businesses.

[:fontawesome-brands-discord: Discord support](https://discord.gg/AeCVP2F8h7){.md-button}

---

## :material-package-down: Installation

1. Drop the resource into your `resources/` folder. Make sure it starts at the **end** of `server.cfg` so all dependencies load first.
2. SQL is executed automatically on server start.
3. Read every comment in the config and tune to your server's needs.

## :material-code-tags: Developer exports

### Offline player

```lua
---@param identifier        string  Identifier of the player to invoice
---@param senderIdentifier  string  Identifier of the player sending the invoice
---@param amount            number  Amount to invoice
---@param reason            string  Reason for the invoice
---@param job               string  Job of the sender
exports["snipe-invoices"]:CreateCustomInvoiceOfflinePlayer(identifier, senderIdentifier, amount, reason, job)
```

### Online player

```lua
---@param source            number  Source of the receiving player
---@param senderIdentifier  string  Identifier of the player sending the invoice
---@param amount            number  Amount to invoice
---@param reason            string  Reason for the invoice
---@param job               string  Job of the sender
exports["snipe-invoices"]:CreateCustomInvoiceOnlinePlayer(source, senderIdentifier, amount, reason, job)
```
