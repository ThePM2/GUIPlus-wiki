> 🔌 **INTEGRATIONS**

# BungeeCord Support

*Send players to other servers in your network*

---

GUIPlus can request a server connection through the `BungeeCord` plugin messaging channel. Use it on a BungeeCord network or a proxy, such as Velocity, configured to accept compatible BungeeCord messages.

## 🔧 Setup

GUIPlus registers the outgoing `BungeeCord` channel automatically. Install GUIPlus on the backend server where the player opens the selector. The player must already be connected through a correctly configured proxy, and the destination must be registered with that proxy.

For BungeeCord setups that use Spigot's BungeeCord forwarding mode, the backend setting is:

```yaml
settings:
  bungeecord: true
```

Configure authentication, forwarding and backend access for your proxy according to its setup instructions. Velocity modern forwarding uses its own backend configuration and requires `settings.bungeecord: false`; follow the [Velocity forwarding guide](https://docs.papermc.io/velocity/player-information-forwarding/). For this action, enable `bungee-plugin-message-channel` in `velocity.toml`, as described in [Velocity plugin messaging](https://docs.papermc.io/velocity/dev/plugin-messaging/#bungeecord-channel-compatibility).

## 🖱 Server Click Event

Use the `server-click-event` to send players to another server:

```yaml
click-events:
  server-click-event:
    server: lobby
```

The `server` field must match the destination name in the proxy configuration. Use a literal configured name: this field is not resolved as a PlaceholderAPI expression. The action sends the clicking player, even when the GUI has a separate target player.

## 🌐 Example: Server Selector

```yaml
id: servers
rows: 3
type: chest
title: '§5Server Selector'
commandAlias: servers
scenes:
  '0':
    delay: 0
    items:
      '1':
        slot: 11
        item: GRASS_BLOCK
        amount: 1
        item-name: '§aSurvival'
        item-lore:
          - ''
          - '§7Classic survival experience'
          - ''
          - '§eClick to join!'
        click-events:
          server-click-event:
            server: survival
      '2':
        slot: 13
        item: IRON_SWORD
        amount: 1
        item-name: '§cPvP Arena'
        item-lore:
          - ''
          - '§7Fight other players'
          - ''
          - '§eClick to join!'
        click-events:
          server-click-event:
            server: pvp
      '3':
        slot: 15
        item: GOLD_BLOCK
        amount: 1
        item-name: '§6Skyblock'
        item-lore:
          - ''
          - '§7Build your island'
          - ''
          - '§eClick to join!'
        click-events:
          server-click-event:
            server: skyblock
```

## 🔗 Combining with Other Events

You can combine the server event with sounds, messages, or conditions:

```yaml
click-events:
  sound-click-event:
    sound: ENTITY_ENDERMAN_TELEPORT
  message:
    message: '§aSending you to Survival...'
  server-click-event:
    server: survival
```

The message says “Sending” because the action requests a connection; GUIPlus does not receive a confirmation that the destination accepted it. A full or offline destination may be refused by the proxy.

## 📊 Server Status and Player Counts

The server action does not fetch counts or check availability. Add text or conditions using placeholders supplied by your network's PlaceholderAPI bridge, such as PAPIProxyBridge. Confirm each placeholder resolves before using it in a requirement.

## 🔍 Troubleshooting

- Confirm that the player joined through the proxy, not directly to the backend.
- Check the destination name and whether the proxy can connect to it.
- Check the proxy's plugin-message compatibility setting and backend forwarding setup.
- Read the proxy logs for an unavailable, full or denied destination.
- If you rename an action entry to something such as `join-survival`, add `type: server-click-event` below it. Unknown action IDs prevent the GUI from loading.

---

| ← Previous | Next → |
|:---|---:|
| [**PlaceholderAPI Placeholders**](PlaceholderAPI-Placeholders.md) | [**Plugin Integrations**](Plugin-Integrations.md) |
