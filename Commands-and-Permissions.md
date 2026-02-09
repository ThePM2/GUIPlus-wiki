> ⚙ **GETTING STARTED**

# Commands & Permissions

*All commands and permission nodes for GUIPlus*

---

> **Tip:** All commands support tab-completion for GUI names and player names.

## Commands

The base command is `/gui`. Aliases: `/guiplus`, `/gplus`, `/gp`

These aliases can be changed in `config.yml` under `command-settings`.

| Command | Description | Permission |
|---------|-------------|------------|
| `/gui` | Opens the GUI management menu | `guiplus.gui` |
| `/gui reload` | Reloads all GUI configurations and plugin settings | `guiplus.gui.reload` |
| `/gui edit <id>` | Opens the in-game editor for a specific GUI | `guiplus.gui.edit` |
| `/gui save` | Manually saves all GUI data to files | `guiplus.gui.save` |
| `/gui open <name> [player]` | Opens a GUI for yourself or another player | `guiplus.gui.open` |
| `/gui openTargeted <name> <target>` | Opens a GUI for another player (player command) | `guiplus.gui.open.targeted` |
| `/gui !!` | Opens the last viewed GUI | `guiplus.gui.openlast` |
| `/gui last` | Opens the last viewed GUI (alias for `!!`) | `guiplus.gui.openlast` |
| `/gui reconvert` | Re-converts legacy inventory format files | `guiplus.gui.reconvert` |

## Permissions

| Permission | Description |
|------------|-------------|
| `guiplus.gui` | Access to the base `/gui` command |
| `guiplus.gui.reload` | Permission to reload configurations |
| `guiplus.gui.edit` | Permission to use the in-game editor |
| `guiplus.gui.save` | Permission to manually save data |
| `guiplus.gui.open` | Permission to open GUIs via command |
| `guiplus.gui.open.others` | Permission to open GUIs for other players |
| `guiplus.gui.open.targeted` | Permission to use the targeted open command |
| `guiplus.gui.openlast` | Permission to open the last viewed GUI |
| `guiplus.gui.reconvert` | Permission to run the legacy converter |

## Per-GUI Permissions

Individual GUIs can have their own permission requirement set in their YAML configuration using the `permission` field:

```yaml
id: vipshop
type: chest
rows: 3
title: §6VIP Shop
permission: myserver.vip.shop
```

Players without `myserver.vip.shop` will not be able to view this GUI.

## Console Usage

The `/gui open <name> <player>` command can be used from the console to open a GUI for a specific player. This is useful for integrating with other plugins via commands.

## Command Aliases per GUI

Each GUI can define its own command alias using the `commandAlias` field. This registers a shortcut command to open that specific GUI:

```yaml
id: shop
commandAlias: shop
```

Players can then use `/shop` to open the GUI directly. The `guiplus.gui.open.others` permission is checked when targeting another player via the alias (e.g., `/shop PlayerName`).

> **Note:** Command aliases do not require `guiplus.gui.open` by default — permission is controlled by the GUI's own `permission` field if set.

---

| ← Previous | Next → |
|:---|---:|
| [**Getting Started**](Getting-Started) | [**Configuration**](Configuration) |
