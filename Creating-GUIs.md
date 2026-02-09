> 🎨 **CREATING GUIS**

# Creating GUIs

*Define custom inventory menus with YAML configuration*

---

GUIs in GUIPlus are defined as YAML files inside the `plugins/GUIPlus/CustomGuis/` folder. Each file represents one GUI menu.

> **Tip:** You can also create GUIs through the in-game editor with `/gui`. The editor generates the YAML for you. See [Getting Started](Getting-Started.md) for the editor workflow.

## 📐 GUI Structure

Every GUI file follows this structure:

```yaml
id: mygui                    # Unique identifier (required)
type: chest                  # Inventory type (required)
rows: 3                      # Number of rows (chest only, 1-6)
title: §6My Menu             # Display title shown to players
commandAlias: mymenu         # Optional shortcut command to open this GUI
commandAliasTarget: OPTIONAL  # OPTIONAL, REQUIRED, or DISABLED
permission: my.perm.node     # Optional permission required to view
disable-scene-animation: false  # Disable animated scene transitions for this GUI
scenes:
  '0':                       # First scene (zero-indexed)
    delay: 0                 # Delay in ticks before this scene opens
    items:
      '1':                   # Item ID (unique within the scene)
        slot: 0              # Inventory slot (0-indexed, left to right, top to bottom)
        item: DIAMOND        # Material type
        amount: 1            # Stack size
        item-name: §bMy Item # Display name (supports color codes)
        item-lore:           # Lore lines
          - §7Description
        item-flags: []       # Item flags (HIDE_ENCHANTS, HIDE_ATTRIBUTES, etc.)
        unbreakable: false   # Whether the item is unbreakable
        click-events:        # Click event actions (see Click Events page)
          message:
            message: §aHello!
        conditions:          # Show conditions (see Conditions page)
          has-permission:
            permission: some.perm
```

## 📦 Inventory Types

| Type | Size | Description |
|------|------|-------------|
| `chest` | 9-54 slots (1-6 rows) | Standard chest inventory. Use the `rows` field to set the size. |
| `dispenser` | 9 slots (3x3) | Dispenser-style layout. Fixed size. |
| `dropper` | 9 slots (3x3) | Dropper-style layout. Fixed size. |
| `hopper` | 5 slots (1x5) | Hopper-style layout. Fixed size. |

## 🔢 Slot Numbers

Slots are numbered starting from `0`, going left to right, top to bottom:

**Chest (3 rows):**
```
 0  1  2  3  4  5  6  7  8
 9 10 11 12 13 14 15 16 17
18 19 20 21 22 23 24 25 26
```

**Hopper:**
```
 0  1  2  3  4
```

**Dispenser / Dropper:**
```
 0  1  2
 3  4  5
 6  7  8
```

## 🏷 Item Properties

Each item in a scene supports the following properties:

| Property | Type | Description |
|----------|------|-------------|
| `slot` | Integer | The inventory slot position (required) |
| `item` | String | Minecraft material name, e.g. `DIAMOND_SWORD` (required) |
| `amount` | Integer | Stack size (default: 1) |
| `item-name` | String | Custom display name with color codes |
| `item-lore` | List | Lines of lore text |
| `item-flags` | List | Bukkit ItemFlags: `HIDE_ENCHANTS`, `HIDE_ATTRIBUTES`, `HIDE_UNBREAKABLE`, etc. |
| `unbreakable` | Boolean | Whether the item should be unbreakable |
| `item-enchants` | Map | Enchantments to apply, e.g. `DURABILITY: 1` |
| `skullBase64` | String | Base64-encoded texture for `PLAYER_HEAD` items (see [Custom Heads](Custom-Heads-and-Skulls.md)) |
| `item-custom-model-data` | Integer | Custom model data value for resource packs |
| `item-model` | String | Item model key as a namespaced ID (Minecraft 1.21.2+ only) |
| `leather-color` | Integer | Leather armor color as an ARGB integer (only for leather armor items) |
| `item-attributes` | Map | Attribute modifiers (see [Item Attributes](#item-attributes) below) |
| `click-events` | Map | Actions triggered on click (see [Click Events](Click-Events.md)) |
| `conditions` | Map | Conditions for showing this item (see [Conditions](Conditions.md)) |
| `conditionFailMessage` | String | Message shown when conditions are not met |

> **Note:** The `sorting-group` property is managed through the in-game editor and is stored in the item's compressed data. It is not a hand-writable YAML field. See [Conditions — Sorting Groups](Conditions.md#sorting-groups) for details.

## ⚔ Item Attributes

You can add attribute modifiers to items using the `item-attributes` property. Each attribute has a modifier with an amount, operation, and UUID:

```yaml
item-attributes:
  GENERIC_ATTACK_DAMAGE:
    damage-boost:
      amount: 10.0
      operation: ADD_NUMBER
      uuid: 550e8400-e29b-41d4-a716-446655440000
```

| Field | Description |
|-------|-------------|
| Attribute key | A Bukkit attribute name (e.g., `GENERIC_ATTACK_DAMAGE`, `GENERIC_MAX_HEALTH`, `GENERIC_MOVEMENT_SPEED`) |
| Modifier key | A unique name for this modifier |
| `amount` | The modifier value |
| `operation` | `ADD_NUMBER`, `ADD_SCALAR`, or `MULTIPLY_SCALAR_1` |
| `uuid` | A UUID to identify this modifier (use any valid UUID) |

## 🎨 Color Codes

GUIPlus supports standard Minecraft color codes using `§` (section sign):

| Code | Color | Code | Format |
|------|-------|------|--------|
| `§0` | Black | `§l` | **Bold** |
| `§1` | Dark Blue | `§m` | ~~Strikethrough~~ |
| `§2` | Dark Green | `§n` | Underline |
| `§3` | Dark Aqua | `§o` | *Italic* |
| `§4` | Dark Red | `§r` | Reset |
| `§5` | Dark Purple | | |
| `§6` | Gold | | |
| `§7` | Gray | | |
| `§8` | Dark Gray | | |
| `§9` | Blue | | |
| `§a` | Green | | |
| `§b` | Aqua | | |
| `§c` | Red | | |
| `§d` | Light Purple | | |
| `§e` | Yellow | | |
| `§f` | White | | |

## 🏷 Command Alias

The `commandAlias` field registers a shortcut command for your GUI:

```yaml
id: shop
commandAlias: shop
```

Players can now use `/shop` to open this GUI directly, instead of `/gui open shop`.

## 🎯 Command Target

The `commandAliasTarget` field controls whether the command alias accepts a player argument:

| Value | Behavior |
|-------|----------|
| `OPTIONAL` | `/shop` opens for self, `/shop PlayerName` opens for target |
| `REQUIRED` | `/shop PlayerName` is required (useful for admin tools) |
| `DISABLED` | No player argument accepted |

## 🛡 Example: Complete Admin Panel

```yaml
id: adminpanel
rows: 3
type: chest
title: §cAdmin Panel
commandAlias: adminpanel
scenes:
  '0':
    delay: 0
    items:
      '1':
        slot: 11
        item: DAMAGED_ANVIL
        amount: 1
        item-name: §cBan User
        item-lore:
          - ''
          - §7Is someone breaking the
          - §7rules? or want them gone?
          - ''
          - §cClick to ban
        item-flags: []
        unbreakable: false
        conditionFailMessage: §cYou can't do that!
        conditions:
          has-permission:
            permission: gui.ban.user
        click-events:
          player-picker-by-player-command:
            command: ban %player% Banned by %executor%
          message:
            message: §bYou have §cbanned §7%player%§b!
      '2':
        slot: 13
        item: ELYTRA
        amount: 1
        item-name: §eKick User
        item-lore:
          - ''
          - §7Want to kick a user?
          - §7you have come to right
          - §7place!
          - ''
          - §eClick to kick
        item-flags: []
        unbreakable: false
        conditionFailMessage: §cYou can't do that!
        conditions:
          has-permission:
            permission: gui.kick.user
        click-events:
          player-picker-by-player-command:
            command: kick %player% You have been kicked by %executor%
          message:
            message: §eYou have kicked §7%player%§e!
      '3':
        slot: 15
        item: LECTERN
        amount: 1
        item-name: §aPardon User
        item-lore:
          - ''
          - §7You can unban/pardon a
          - §7user!
          - ''
          - §bClick to pardon
        item-flags: []
        unbreakable: false
        conditions:
          has-permission:
            permission: gui.pardon.user
        click-events:
          offline-player-picker-command:
            command: unban %player%
          message:
            message: §aYou have successfully unbanned §7%player%§a!
```

---

| ← Previous | Next → |
|:---|---:|
| [**Configuration**](Configuration.md) | [**Scenes**](Scenes.md) |
