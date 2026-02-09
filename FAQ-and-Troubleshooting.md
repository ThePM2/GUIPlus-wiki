> ❓ **HELP**

# FAQ & Troubleshooting

*Common questions and solutions for GUIPlus*

---

## Frequently Asked Questions

### How do I create a new GUI?

There are two ways:

1. **In-Game Editor:** Run `/gui` to open the management menu, then create a new GUI through the editor interface.
2. **YAML File:** Create a new `.yml` file in `plugins/GUIPlus/CustomGuis/` and define your GUI structure. See [Creating GUIs](Creating-GUIs).

### How do I reload my changes?

Run `/gui reload` to reload all GUI configurations and plugin settings. This does not require a server restart.

### Can I use PlaceholderAPI placeholders?

Yes. GUIPlus fully supports PlaceholderAPI. Any PAPI placeholder can be used in item names, lore, click event parameters, and condition values. See [PlaceholderAPI Placeholders](PlaceholderAPI-Placeholders).

### How do I make items visible only to certain players?

Use [conditions](Conditions) on items. For example, to show an item only to players with a specific permission:

```yaml
conditions:
  has-permission:
    permission: myserver.vip
conditionFailMessage: §cVIP only!
```

### How do I create a multi-page menu?

Use [scenes](Scenes). Each scene acts as a separate page, and players navigate between them using `next-scene-click` and `previous-scene-click` events.

### Can I use GUIPlus with BungeeCord/Velocity?

Yes. Use the `server-click-event` to send players to other servers. See [BungeeCord Support](BungeeCord-Support).

### How do I store per-player data?

Use the `save-player-info` click event to save data and `%GUIPlus_player_info_<field>%` to retrieve it. See [Player Data](Player-Data).

### Can I open GUIs from other plugins?

Yes. Other plugins can use `/gui open <name> <player>` as a console command to open any GUIPlus GUI for a player. This works with any command-executing plugin.

### How do I open a GUI for another player?

Use `/gui open <name> <player>` (requires `guiplus.gui.open` permission) or `/gui openTargeted <name> <player>` (requires `guiplus.gui.open.targeted` permission). For command aliases (e.g., `/shop PlayerName`), the `guiplus.gui.open.others` permission is required.

### How do I convert from DeluxeMenus?

Place your DeluxeMenus files in `plugins/GUIPlus/DeluxeMenus/` and restart. See [DeluxeMenus Converter](DeluxeMenus-Converter).

### How do I add sound effects to buttons?

Use the `sound-click-event` click event alongside your other events. Sounds play at the player's location:

```yaml
click-events:
  sound-click-event:
    sound: UI_BUTTON_CLICK
    volume: 1.0
    pitch: 1.0
  message:
    message: §aButton clicked!
```

See [Click Events — Sound](Click-Events#sound) for all options.

### How do I make a left-click / right-click do different things?

Use the `clickType` property on each click event to restrict it to a specific mouse button:

```yaml
click-events:
  buy-action:
    message: §aPurchased!
    clickType: LEFT
  info-action:
    message: §7This costs $500
    clickType: RIGHT
```

Valid click types: `LEFT`, `RIGHT`, `MIDDLE`, `SHIFT_LEFT`, `SHIFT_RIGHT`, `NONE` (any click).

### Can I use GUIPlus with ItemsAdder or Oraxen?

Yes. Use custom model data (`item-custom-model-data`) for custom item textures, and console commands to give custom items. Font image placeholders work in item lore if the respective PAPI expansion is installed. See [Plugin Integrations](Plugin-Integrations) for details.

### How do I open a GUI from an NPC?

If using Citizens, add a command to the NPC: `/npc command add -p gui open <name>`. Any NPC plugin that supports running commands on interaction works similarly. See [Plugin Integrations — Citizens](Plugin-Integrations#citizens--npcs).

### How do I add a cooldown to a button?

Use the `cooldown` condition on the item. The cooldown value is in **milliseconds**:

```yaml
conditions:
  cooldown:
    cooldown: 60000
    id: my-cooldown
```

This prevents the item from being clicked more than once per 60 seconds. See [Conditions — Cooldown](Conditions#cooldown).

### Are there premade GUI templates I can use?

Yes. See [Premade Configurations](Premade-Configurations) for ready-to-use templates including server rules, gamemode selectors, social links menus, daily rewards, and more.

---

## Troubleshooting

### GUI doesn't open

- Check that the player has the `guiplus.gui.open` permission
- Verify the GUI ID matches exactly (case-sensitive)
- Check console for errors after running `/gui reload`
- Ensure the GUI file has valid YAML syntax (use a YAML validator if unsure)

### Items not showing

- Verify the material name is a valid Minecraft material (e.g., `DIAMOND_SWORD`, not `diamond sword`)
- Check if the item has conditions that the player doesn't meet
- Ensure the `slot` number is within the GUI's size range
- Check for YAML indentation errors in the item definition

### Placeholders not updating

- Verify PlaceholderAPI is installed and running
- Check that the placeholder expansion is installed (e.g., run `/papi ecloud download Player`)
- Ensure the placeholder format is correct: `%placeholder_name%`

### Click events not working

- Ensure the click event ID is correct (see [Click Events](Click-Events))
- Check if there's a `clickType` restriction that doesn't match how the player is clicking
- If using `has-money` or other economy events, verify Vault and an economy plugin are installed
- Check the console for error messages when clicking

### Chat fetcher not responding

- Make sure `disable-commands-chat-fetcher` and `disable-guis-chat-fetcher` are set correctly in `config.yml`
- Players can type `exit` to cancel the chat fetcher
- Check the `conditionFailMessage` — input validation might be failing silently

### Skull textures not displaying

- Verify the Base64 string is complete and valid
- For `%self_base64%`, ensure PlaceholderAPI is installed
- The item material must be `PLAYER_HEAD`

### Plugin not loading

- Check that you're running Java 17 or higher
- Verify your server is running Spigot or Paper 1.16+
- Check the console for error messages during startup
- Ensure the JAR file is not corrupted (re-download if needed)

### Config changes not applying

- Use `/gui reload` after making changes
- Some settings (like command aliases in `config.yml`) require a full server restart
- Verify your YAML syntax — a single indentation error can break the entire file

### Performance issues

- Reduce the number of placeholders in item lore (each is evaluated every second)
- Avoid using many open GUIs simultaneously with dynamic content
- Keep individual GUIs focused — split large menus into separate GUIs linked by commands
- See [Tips & Best Practices — Performance](Tips-and-Best-Practices#performance) for detailed optimization guidance

### Economy events not working (money-give, money-remove, has-money)

- Verify **Vault** is installed and loaded (check `/plugins`)
- Verify you have an economy plugin installed (EssentialsX, CMI, etc.)
- Check that Vault is detecting your economy plugin: look for `[Vault] Hooking` messages in the startup log
- If using `has-money` with a placeholder value (e.g., `'%input%'`), make sure the value resolves to a valid number

### Chat fetcher closes but nothing happens

- Check that the nested `click-events` inside the `chat-fetcher` are correctly indented under the chat fetcher block
- Verify your conditions — if any condition fails, the `conditionFailMessage` is shown and nested events don't execute
- Check the console for errors after entering input
- Make sure the `%input%` placeholder is correctly placed in your nested events

### Items flickering or disappearing

- This usually happens when conditions are evaluated and items alternate between showing and hiding
- Check that condition values (especially placeholder-based ones) return stable results
- Ensure your YAML indentation is correct — misplaced conditions can cause unexpected behavior

### Command alias not registering

- Command aliases defined via `commandAlias` require a **full server restart** (not just `/gui reload`)
- Check that no other plugin is already using the same command name
- Verify the alias in the GUI file is a simple string without spaces or special characters

---

## Getting Help

If you're still having issues:

1. Enable `show-stacktrace: true` in `config.yml` for detailed error messages
2. Enable `debug.enabled: true` for verbose logging
3. Check the console output after reproducing the issue
4. Review [Tips & Best Practices](Tips-and-Best-Practices) for common pitfalls
5. Join the plugin's Discord server for community support

---

| ← Previous | Next → |
|:---|---:|
| [**DeluxeMenus Converter**](DeluxeMenus-Converter) | [**Developer API**](Developer-API) |
