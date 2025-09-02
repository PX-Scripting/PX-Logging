# PX-Logging

Discord logging system for FiveM servers with automatic event detection and rich embed support.

## Features

- Automatic player connection/disconnection tracking
- Chat message logging with command detection
- Death and kill logging with weapon details
- Vehicle spawning and deletion monitoring
- Admin action tracking with Steam ID logging
- Resource start/stop notifications
- Anti-cheat detection integration
- Speed and teleportation monitoring
- Economy transaction logging
- Property and inventory action tracking

## Installation

1. Download and extract to your `resources` folder
2. Add `ensure PX-Logging` to your `server.cfg`
3. Configure your Discord webhook in `config.lua`
4. Restart your server

## Quick Setup

Edit `config.lua` and add your Discord webhook URL:

```lua
Config.Discord = {
    webhook = 'YOUR_DISCORD_WEBHOOK_URL_HERE',
    botName = 'PX Logger',
    embedColor = 3447003,
    maxMessagesPerMinute = 30
}
```

## Configuration

### Enable/Disable Events

```lua
Config.AutoLogging = {
    playerConnect = { enabled = true },
    playerDisconnect = { enabled = true },
    chatMessages = { enabled = true },
    playerDeath = { enabled = true },
    vehicleSpawn = { enabled = true },
    adminActions = { enabled = true },
    resourceStart = { enabled = true },
    explosions = { enabled = true },
    teleportation = { enabled = true },
    speedDetection = { enabled = true }
}
```

### Privacy Options

```lua
Config.Privacy = {
    hashSteamIds = false,
    hashIPs = true,
    chatBlacklist = {
        'password',
        'private'
    }
}
```

## Discord Examples

### Player Connection
```
👋 Player Connected
John_Doe is connecting to the server

Steam ID: steam:110000...
Discord ID: @JohnDoe#1234
Ping: 45ms
```

### Player Death
```
💀 Player Death
Mike_Smith killed John_Doe

Killer: Mike_Smith
Victim: John_Doe  
Weapon: Assault Rifle
Distance: 25m
```

### Admin Action
```
🛡️ Admin Action
AdminName performed admin action: ban

Admin: AdminName (ID: 2)
Target: PlayerName (ID: 15)
Details: Cheating - 7 days
Time: 14:32:15
```

## Integration

### Basic Usage

```lua
-- Log custom admin actions
TriggerServerEvent('PX-Logging:AdminAction', 'teleport', targetId, 'Teleported to bank')

-- Log economy transactions  
exports['PX-Logging']:logEconomyTransaction('withdraw', 50000, 'ATM withdrawal')

-- Log property actions
exports['PX-Logging']:logPropertyAction('purchase', propertyId, 'Bought for $75,000')
```

### ESX Integration

```lua
-- In your ESX scripts
RegisterServerEvent('esx:playerLoaded')
AddEventHandler('esx:playerLoaded', function(playerId, xPlayer)
    TriggerEvent('PX-Logging:PlayerAction', 'character_loaded', playerId, xPlayer.getName())
end)
```

### QBCore Integration

```lua
-- In your QBCore scripts
RegisterNetEvent('QBCore:Server:PlayerLoaded', function(Player)
    TriggerServerEvent('PX-Logging:EconomyTransaction', 'login_bonus', 1000, 'Daily login reward')
end)
```

## Available Export Functions

| Function | Description | Example |
|----------|-------------|---------|
| `logAdminAction` | Log admin actions | `exports['PX-Logging']:logAdminAction('kick', playerId, 'AFK')` |
| `logEconomyTransaction` | Log money transactions | `exports['PX-Logging']:logEconomyTransaction('transfer', 5000, 'Player trade', targetId)` |
| `logPropertyAction` | Log property interactions | `exports['PX-Logging']:logPropertyAction('raid', propertyId, 'Police raid')` |
| `logInventoryAction` | Log item transactions | `exports['PX-Logging']:logInventoryAction('give', 'weapon_pistol', 1, 2500, targetId)` |
| `logEmergencyAction` | Log police/EMS actions | `exports['PX-Logging']:logEmergencyAction('arrest', suspectId, 'Armed robbery')` |
| `logGangAction` | Log gang activities | `exports['PX-Logging']:logGangAction('territory_capture', 'Grove Street', 'Captured Ballas turf')` |

## Anti-Cheat Integration

The system includes built-in detection for:

- Speed hacking (configurable thresholds)
- Teleportation (distance-based detection)  
- Noclip usage
- God mode
- Weapon modifications
- Infinite ammo

```lua
-- Manual anti-cheat reporting
exports['PX-Logging']:logAnticheatDetection('godmode', 'Health remained at 200 for 60 seconds')
```

## Advanced Configuration

### Multiple Webhooks

Send different events to different Discord channels:

```lua
Config.AutoLogging.playerConnect.webhook = 'https://discord.com/api/webhooks/...' -- Join/Leave channel
Config.AutoLogging.adminActions.webhook = 'https://discord.com/api/webhooks/...'  -- Admin channel
Config.AutoLogging.playerDeath.webhook = 'https://discord.com/api/webhooks/...'   -- PvP channel
```

### Performance Tuning

```lua
Config.Performance = {
    batchSize = 20,
    flushInterval = 3000,
    maxQueueSize = 500,
    cooldowns = {
        playerDeath = 5000,
        chatMessage = 1000,
        vehicleSpawn = 3000
    }
}
```

## Troubleshooting

**Webhook not working:**
- Verify webhook URL is complete and correct
- Check Discord channel permissions
- Enable debug mode: `Config.Debug = true`

**Rate limiting:**
- Reduce `maxMessagesPerMinute` in config
- Increase cooldown timers
- Disable noisy events like `vehicleDelete`

**Missing events:**
- Check if event is enabled in `Config.AutoLogging`
- Verify resource started after framework
- Check server console for errors

## Requirements

- FiveM Server
- Discord Webhook URL

## Support

For issues and feature requests, create an issue on GitHub or contact PX-Scripts.

## License

This project is licensed under the MIT License.
