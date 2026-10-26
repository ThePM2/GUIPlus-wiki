> 💬 **INTERACTIVE**

# Personal Storage

*Give each player saved item storage with configurable sizes.*

---

GUIPlus storage holds real player items. It is separate from decorative menu slots and from values saved with [Player Data](Player-Data.md).

## 📦 Open Storage Directly

The default command is `/storage`, with `/vault`, `/playervault` and `/pv` as aliases. In `config.yml`:

```yaml
command-settings:
  storage-command:
    name: storage
    aliases: [vault, playervault, pv]
    description: 'Opens your personal storage'
    storage-id: vault
    title: "&8%player%'s Storage"
    rows: 3
    permission-rows:
      - 'guiplus.storage.vip:6'
```

Restart after changing these command settings. Players get three rows; a player with `guiplus.storage.vip` gets six. With several matching permissions, the highest size wins.

Set `name: ''` to disable the direct command. If `/vault` conflicts with another plugin, remove that alias or use `/storage`.

## 🖱 Add a Storage Button

This fragment belongs inside an item's `click-events`:

```yaml
open-storage:
  storage-id: vault
  title: "&8%player%'s Storage"
  rows: 3
  permission-rows:
    - 'guiplus.storage.vip:6'
```

It opens the clicking player's pool. It does not open the selected target's storage in a targeted menu. The command and button share the same saved inventory when they use the same `storage-id`.

> **Note:** The action's default pool is `default`; the shipped command uses `vault`. Specify `storage-id: vault` if you want the two entry points to show the same items.

## 🔑 Storage Pools and Sizes

Each player UUID has a separate inventory for each storage ID. Use simple IDs such as `vault` or `backpack_2`; letters, digits, underscores and hyphens are kept, while other characters are replaced with underscores.

Chest sizes are 1–6 rows. If a player's allowed size becomes smaller while higher slots still contain items, GUIPlus keeps enough rows visible to withdraw those saved items. Slots beyond the new allowance accept withdrawals but not new deposits. Items are not thrown away when a permission tier changes.

`permission-rows` must be a list of `permission:rows` strings. Dotted permission nodes are not suitable as nested YAML map keys here.

## 👥 Opening Another Player's Storage

```text
/storage Alex
```

The player must be online. A player using the command with a supplied name needs `guiplus.storage.others`; console may use it with an online player name. The inventory opens for Alex on Alex's screen. It is not an offline staff inspection command.

## 💾 Saved Items

Storage is saved in `plugins/GUIPlus/playerStorage/<uuid>.yml`. Back up this folder with the rest of GUIPlus and Minecraft player data. Do not edit an open player's storage file by hand.

If the file cannot be read, GUIPlus refuses to open an empty replacement. Check the console and repair or restore the file. If only an individual saved item is unreadable on the current server version, its data is retained; avoid downgrading servers with newer items in storage.

See [Updating & Backups](Updating-and-Backups.md) before changing server or plugin versions.
