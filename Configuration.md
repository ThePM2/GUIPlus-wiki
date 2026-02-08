# Configuration

The main configuration file is located at `plugins/GUIPlus/config.yml`. Below is every option explained.

## Full Default Configuration

```yaml
# Please do not modify the 'v' field, it is used to detect updates
# If modified, it can cause data loss!
v: ${project.version}

# Disables automatic backups, the backups are made when the plugin is upgraded to a newer version
disable-automatic-backup: false

# Used for the test server, not meant for production or use in any other way
test-mode: false
test-mode-menus: []

# Enable to show full errors when something fails, this is currently W.I.P
show-stacktrace: false

# Disables the "/gui !!" message
disable-last-gui-message: false

debug:
  enabled: false

chat-fetcher-exit-message-cooldown: 10 # unit is seconds

gui-settings:
  # Anything not mentioned here will have -1 row, helpful in cases such as dispenser, hopper, dropper, etc.
  # If you try to set rows for a menu type such as dispenser, hopper, etc, it would have no effect on them
  default-rows:
    chest: 3
  # Delay in ticks to update placeholders in the gui
  update-delay: 1

# disable the commands while in chat fetcher
disable-commands-chat-fetcher: true
disable-guis-chat-fetcher: true

disable-scenes-help: false
disable-editor-close-message: true

# Disables embedding data within item's nbt, this is used to store custom data such as
# lore pages, clicks, conditions, etc, if disabled it will not save within the item
# so if you were to misclick an item and pick it up, all data for that item would be lost
disable-item-data-saving: false

# This enables a feature that if a file such as for example, "messages.yml" is deleted and
# the plugin is reloaded (not server restarts), the file will be recreated with all it's contents
# but this DOES NOT apply to GUI configurations.
data-protection: false

command-settings:
  gui-command:
    name: "gui"
    aliases: [ "guiplus", "gplus", "gp" ]
    description: "Main command for the GUIPlus plugin"
```

## Options Explained

### Version Field
```yaml
v: <version>
```
> **Warning:** Do not modify this field. It is auto-populated at build time and used internally to detect plugin upgrades and trigger automatic configuration migrations. Changing it can cause data loss.

### Automatic Backups
```yaml
disable-automatic-backup: false
```
When `false` (default), the plugin creates a backup of your configuration files whenever you upgrade to a newer version of GUIPlus. Set to `true` to disable this behavior.

### Test Mode
```yaml
test-mode: false
test-mode-menus: []
```
Intended for development/test servers only. When enabled, the specified menus in `test-mode-menus` are reserved for testing purposes.

### Stack Trace Display
```yaml
show-stacktrace: false
```
When enabled, full Java stack traces are shown in the console when errors occur. Useful for debugging or when reporting issues to the developer.

### Last GUI Message
```yaml
disable-last-gui-message: false
```
Controls whether the "You can reopen your last GUI with `/gui !!`" notification is shown to players. Set to `true` to suppress this message.

### Debug Mode
```yaml
debug:
  enabled: false
  priority: LOW
```
Enables verbose debug logging to the console. Only enable when troubleshooting issues.

The `priority` field (not in default config, defaults to `LOW`) controls which debug messages are shown. Valid values: `LOW` (all messages), `MEDIUM`, `HIGH` (most important only).

### Chat Fetcher Exit Cooldown
```yaml
chat-fetcher-exit-message-cooldown: 10
```
The number of seconds before the "type `exit` to cancel" message is re-shown to a player while they are in the chat fetcher input mode.

### GUI Settings
```yaml
gui-settings:
  default-rows:
    chest: 3
  update-delay: 1
```
- **`default-rows.chest`** — The default number of rows when creating a new chest GUI (1-6). Inventory types like dispenser, hopper, and dropper have fixed sizes and ignore this setting.
- **`update-delay`** — Defined in the default config but currently not read by the plugin. The placeholder/lore update interval is hardcoded at 20 ticks (1 second). Changing this value has no effect.

### Chat Fetcher Command/GUI Blocking
```yaml
disable-commands-chat-fetcher: true
disable-guis-chat-fetcher: true
```
- **`disable-commands-chat-fetcher`** — When `true`, players cannot execute commands while the plugin is waiting for their chat input.
- **`disable-guis-chat-fetcher`** — When `true`, players cannot open other GUIs while the plugin is waiting for their chat input.

### Scene Help
```yaml
disable-scenes-help: false
```
Controls whether helpful text about scenes is shown in the editor. Set to `true` to hide it.

### Editor Close Message
```yaml
disable-editor-close-message: true
```
When `true`, suppresses the notification shown when closing the GUI editor.

### Item Data Saving (NBT)
```yaml
disable-item-data-saving: false
```
GUIPlus embeds click event and condition data within item NBT tags. If disabled, accidentally picking up items from the GUI editor will cause data loss for those items.

### Data Protection
```yaml
data-protection: false
```
When enabled, if a core file like `messages.yml` is deleted and the plugin is reloaded, the file will be automatically recreated. This does **not** apply to GUI configuration files in `CustomGuis/`.

### Command Settings
```yaml
command-settings:
  gui-command:
    name: "gui"
    aliases: [ "guiplus", "gplus", "gp" ]
    description: "Main command for the GUIPlus plugin"
```
Customize the main plugin command name, its aliases, and description. You can also add `permission` and `permissionMessagePath` keys to override the base permission required and the denial message path (from `messages.yml`). Changes require a **server restart** (not just a reload).

---

[Previous: Commands & Permissions](Commands-and-Permissions) | [Next: Creating GUIs](Creating-GUIs)
