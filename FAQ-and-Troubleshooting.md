# FAQ & Troubleshooting

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

- Reduce the number of placeholders in item lore
- Avoid using many open GUIs simultaneously with dynamic content

---

## Getting Help

If you're still having issues:

1. Enable `show-stacktrace: true` in `config.yml` for detailed error messages
2. Enable `debug.enabled: true` for verbose logging
3. Check the console output after reproducing the issue
4. Join the plugin's Discord server for community support

---

[Previous: DeluxeMenus Converter](DeluxeMenus-Converter) | [Next: Developer API](Developer-API)
