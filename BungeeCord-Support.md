# BungeeCord Support

GUIPlus supports sending players to other servers on your BungeeCord or Velocity network using the Server click event.

## Setup

BungeeCord support works out of the box. GUIPlus registers the `BungeeCord` plugin messaging channel automatically when the plugin starts.

Make sure your `spigot.yml` has BungeeCord mode enabled:

```yaml
settings:
  bungeecord: true
```

## Server Click Event

Use the `server-click-event` to send players to another server:

```yaml
click-events:
  server-click-event:
    server: lobby
```

The `server` field must match the server name as configured in your BungeeCord/Velocity `config.yml`.

## Example: Server Selector

```yaml
id: servers
rows: 3
type: chest
title: §5Server Selector
commandAlias: servers
scenes:
  '0':
    delay: 0
    items:
      '1':
        slot: 11
        item: GRASS_BLOCK
        amount: 1
        item-name: §aSurvival
        item-lore:
          - ''
          - §7Classic survival experience
          - ''
          - §eClick to join!
        click-events:
          server-click-event:
            server: survival
      '2':
        slot: 13
        item: IRON_SWORD
        amount: 1
        item-name: §cPvP Arena
        item-lore:
          - ''
          - §7Fight other players
          - ''
          - §eClick to join!
        click-events:
          server-click-event:
            server: pvp
      '3':
        slot: 15
        item: GOLD_BLOCK
        amount: 1
        item-name: §6Skyblock
        item-lore:
          - ''
          - §7Build your island
          - ''
          - §eClick to join!
        click-events:
          server-click-event:
            server: skyblock
```

## Combining with Other Events

You can combine the server event with sounds, messages, or conditions:

```yaml
click-events:
  sound-click-event:
    sound: ENTITY_ENDERMAN_TELEPORT
  message:
    message: §aSending you to Survival...
  server-click-event:
    server: survival
```

> **Tip:** Add conditions to show server status or player counts using PlaceholderAPI placeholders from plugins like `PAPIProxyBridge`.

---

[Previous: PlaceholderAPI Placeholders](PlaceholderAPI-Placeholders) | [Next: DeluxeMenus Converter](DeluxeMenus-Converter)
