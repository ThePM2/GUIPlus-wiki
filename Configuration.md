> ⚙ **GETTING STARTED**

# Configuration

*Main settings, command names and automatic backups.*

---

The main file is `plugins/GUIPlus/config.yml`. Save an editor draft before changing files, then use `/gui reload` for settings and menu changes. Restart after changing command registration settings.

## 📄 Default Configuration

```yaml
# Please do not modify the 'v' field, it is used to detect updates
# If modified, it can cause data loss!
v: 4.0.0

# Disables automatic backups. They are made when the plugin is upgraded to a newer version, and when the
# server's Minecraft version changes, because data written on one version cannot always be read on another
disable-automatic-backup: false

# Used for the test server, not meant for production or use in any other way
test-mode: false
test-mode-menus: []

# Enable to show full errors when something fails, this is currently W.I.P
show-stacktrace: false

# Prevent players from moving items into or out of open GUIPlus player GUIs
prevent-pickup: true

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
  # Delay in ticks for dynamic text refresh; bounded to 1..1200. Static slots are skipped.
  update-delay: 20
  # When switching between GUIs/scenes that share the same size and type,
  # replace the items in the already-open window instead of closing and reopening it.
  # This removes the flicker and keeps the player's cursor position. Set to false to
  # always use the old close-and-reopen behavior.
  seamless-switching: true
  # When a seamless switch needs a different window title, update it in place
  # (Paper servers only — on Spigot, or with this disabled, such switches use the
  # old close-and-reopen behavior so the title is never stale).
  seamless-title-updates: true

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
  # Opens a player's personal storage. Set name to "" to disable this command.
  storage-command:
    name: "storage"
    aliases: [ "vault", "playervault", "pv" ]
    description: "Opens your personal storage"
    storage-id: "vault"               # which saved storage pool to open
    title: "&8%player%'s Storage"
    rows: 3                           # default size (1-6); raised by the highest matching permission below
    permission-rows:                  # list of "permission:rows"; highest matching tier wins
      - "guiplus.storage.vip:6"

# A successful version-change backup is required before migrating stored data.
# Recurring backups are checked by the five-minute maintenance task; 0 disables them.
backup:
  interval-hours: 24
  retained-copies: 14
```

## 🔧 Main Options

| Setting | What it changes |
|---------|-----------------|
| `v` | GUIPlus's saved version marker. Leave it alone; it is used when updating data. |
| `disable-automatic-backup` | Disables version-change and recurring GUIPlus backups when `true`. Leave `false` for normal servers. |
| `prevent-pickup` | Prevents moving items into or out of ordinary player menus. Personal storage has its own item handling. |
| `show-stacktrace` | Shows full error details for troubleshooting. |
| `disable-last-gui-message` | Hides the `/gui !!` reminder. |
| `disable-editor-close-message` | Hides the editor-close notification. |
| `disable-scenes-help` | Hides scene help in the editor. |
| `disable-item-data-saving` | Stops embedding editor behavior in item data. Keep `false` when working with editor items. |
| `data-protection` | Recreates certain deleted core files from the in-memory copy during reload. It does not restore deleted GUI files. |
| `test-mode`, `test-mode-menus` | Development settings. Leave disabled on a normal server. |

## ⏱ Refresh and Animation

`gui-settings.update-delay` sets dynamic text refresh in **ticks**, bounded to 1–1200. The default is `20`, about one second. Static slots are skipped. Animated lore uses its own page delays; see [Animations](Animations.md).

`gui-settings.seamless-switching` reuses a window when the next GUI or scene has the same size and type. `seamless-title-updates` also changes the title in place on Paper. On Spigot, a title change uses the normal close-and-open path.

`gui-settings.default-rows.chest` controls the default size of a new chest menu. Dispenser, dropper and hopper menus have fixed sizes.

## 💬 Chat Input

- `disable-commands-chat-fetcher` blocks commands while GUIPlus is waiting for chat input.
- `disable-guis-chat-fetcher` blocks opening another GUI during the prompt.
- `chat-fetcher-exit-message-cooldown` is the reminder interval in **seconds**. It is not the button cooldown.

Validation rules belong to the prompt or flow field. See [Input Rules](Input-Rules.md).

## 🏷 Commands and Storage

`command-settings.gui-command` changes the main command name, aliases and description. An optional `permission` adds a root permission gate; `permissionMessagePath` chooses its denial message from `messages.yml`.

`command-settings.storage-command` controls the direct personal-storage command. Set `name: ''` to disable it. `storage-id` identifies the saved pool, `rows` supplies the default size and `permission-rows` gives larger sizes to selected players:

```yaml
permission-rows:
  - 'guiplus.storage.vip:6'
```

Use a list of `permission:rows` strings, not a map with dotted permission keys. The highest matching size wins. See [Personal Storage](Personal-Storage.md) for a complete example.

## 💾 Backups

`backup.interval-hours` defaults to `24`. The maintenance task checks for a due backup every five minutes; `0` disables the recurring schedule. `backup.retained-copies` defaults to `14` and is bounded to 1–1000.

A version-change backup runs before stored data is migrated, unless automatic backups are disabled. Backups include menus, editor layouts, player storage, rewards, shared components, warp catalogs and transaction records. They do not replace a server backup of Minecraft player data. See [Updating & Backups](Updating-and-Backups.md).

## 🐛 Debug Messages

```yaml
debug:
  enabled: true
  priority: LOW
```

The current setting includes message levels cumulatively: `LOW` shows LOW messages, `MEDIUM` shows LOW and MEDIUM, and `HIGH` shows all three levels. Turn debug off after troubleshooting. `/gui diagnostics` provides session and runtime counts without changing this setting.

---

| ← Previous | Next → |
|:---|---:|
| [**Commands & Permissions**](Commands-and-Permissions.md) | [**Creating GUIs**](Creating-GUIs.md) |
