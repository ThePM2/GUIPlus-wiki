> 🎨 **CREATING GUIS**

# Creating GUIs

*Build inventory menus with the in-game editor or YAML.*

---

Each menu is a `.yml` file in `plugins/GUIPlus/CustomGuis/`. Run `/gui` to create one in game, or start with the example below.

## 📐 GUI Structure

```yaml
id: server_menu
type: chest
rows: 3
title: '&6Server Menu'
commandAlias: servermenu
commandAliasTarget: DISABLED
disable-scene-animation: true
scenes:
  '0':
    delay: 0
    items:
      '1':
        slot: 13
        item: DIAMOND
        amount: 1
        item-name: '&bWelcome'
        item-lore:
          - '&7Click to read the server rules.'
        click-events:
          message:
            message: '&aBe kind and keep chat friendly.'
```

Save it as `server_menu.yml`, run `/gui reload`, then `/gui open server_menu`. The optional `permission` at the menu root controls who can open it.

`commandAlias` creates a shortcut. `commandAliasTarget` controls what a supplied player name means; see [Commands & Permissions](Commands-and-Permissions.md). Use `disable-scene-animation: true` for menus whose pages are changed by buttons. Leave it `false` for a timed scene sequence.

## 📦 Inventory Types and Slots

| Type | Size |
|------|------|
| `chest` | 1–6 rows, 9 slots per row. |
| `dispenser` | 9 slots, 3×3. |
| `dropper` | 9 slots, 3×3. |
| `hopper` | 5 slots, one row. |

Slots start at `0`, left to right and top to bottom. A three-row chest uses:

```text
 0  1  2  3  4  5  6  7  8
 9 10 11 12 13 14 15 16 17
18 19 20 21 22 23 24 25 26
```

The item IDs inside `items` identify entries; `slot` determines where each appears.

## 🏷 Item Properties

| Property | Purpose |
|----------|---------|
| `slot`, `item`, `amount` | Position, material and stack size. |
| `item-name`, `item-lore` | Display name and lore lines. |
| `item-lore-data` | Additional lore pages and their delays. See [Animations](Animations.md). |
| `item-flags` | Item flags such as `HIDE_ENCHANTS` or `HIDE_ATTRIBUTES`. |
| `unbreakable` | Whether the item is unbreakable. |
| `item-enchants` | Enchantment names mapped to levels. |
| `skullBase64` | A custom head texture. See [Custom Heads & Skulls](Custom-Heads-and-Skulls.md). |
| `item-custom-model-data` | Numeric custom model data for a resource pack. |
| `item-custom-model-data-component` | Structured floats, strings, flags and colors on supporting server versions. Copy an actual item in the editor to retain these values. |
| `item-model` | A namespaced item-model key on versions supporting it. |
| `leather-color` | An ARGB integer for leather armor. |
| `item-attributes` | Attribute modifiers. |
| `showConditions` | Requirements for displaying this item. |
| `conditions` | Requirements checked before its click actions run. |
| `conditionFailMessage` | Message when a click requirement fails. |
| `click-events` | Actions for the item. |
| `nestedItems` | Fallback appearances when the parent's visibility requirements fail. |
| `sorting-group` | Group ID for compacting eligible items into the group's slots. |
| `state-button` | Locked, Ready and Owned definitions. See [Button States](Button-States.md). |
| `shared-component` | An explicit shared template link. See [Shared Components](Shared-Components.md). |

### Enchantments and Attributes

These fragments belong inside an item:

```yaml
item-enchants:
  UNBREAKING: 1
item-flags:
  - HIDE_ENCHANTS
unbreakable: true
```

For an attribute modifier:

```yaml
item-attributes:
  ATTACK_DAMAGE:
    damage-boost:
      amount: 2.0
      operation: ADD_NUMBER
      uuid: '550e8400-e29b-41d4-a716-446655440000'
      slot: HAND
```

Use names supported by the server version. The in-game editor can copy an existing item's metadata, including attributes and custom item data. Saved items may also contain `serialized-item`, `item-envelope` and `item-nbt-data`; keep those fields together rather than hand-editing encoded data. To change a copied custom item, use the editor so its stored appearance remains consistent.

## 🎨 Colors and Titles

Legacy text supports `&` and `§` color codes, plus hex colors such as `&#55aaff`:

```yaml
title: '&8Server Menu'
```

Quote strings containing `&`, `%`, `#` or a colon. A color code at the start of an unquoted YAML value can be read as YAML syntax.

### MiniMessage Inventory Titles

On Paper, choose **Title Format** in the menu editor, then enter a MiniMessage title. In YAML, set both root fields:

```yaml
title-format: MINIMESSAGE
title: '<gradient:#55aaff:#aa55ff>Server Menu</gradient>'
```

For a resource-pack font:

```yaml
title-format: MINIMESSAGE
title: '<font:myserver:menu>Server Menu</font>'
```

Players need the matching resource pack for that font. MiniMessage title mode preserves the component on supported Paper servers; the legacy fallback on other servers cannot preserve custom font components. Ordinary menus default to `LEGACY`. This setting changes the inventory title, not every text field in the plugin.

## ✅ Visibility and Click Requirements

Put this inside a staff item to hide it from other players and check the permission again when clicked:

```yaml
showConditions:
  has-permission:
    permission: myserver.staff
conditions:
  has-permission:
    permission: myserver.staff
conditionFailMessage: '&cStaff only.'
```

The maps serve different purposes. `showConditions` selects the appearance; `conditions` guards execution. A scene's `open-conditions` controls admission to that scene. See [Conditions](Conditions.md).

## 🧩 Nested Items, Copies and Shared Buttons

Use **Nested Items** for a fallback at the same slot. For example, show an unlocked button to VIPs and a gray information item to everyone else. Sorting groups instead pack visible entries into the group's authored slots.

A normal copied item is independent. Choose [Shared Components](Shared-Components.md) when several menus should use one button definition. Choose [Button States](Button-States.md) for a button that changes between Locked, Ready and Owned.

### Copy a Menu

The menu settings also offer **Design copy** and **Full copy**. Create a different destination menu with no scenes, choose a copy mode from the source menu and enter that destination ID. An existing nonempty menu is not merged or overwritten. Review the resulting buttons and save the destination; check any state buttons or shared links when adapting a copied design.

---

| ← Previous | Next → |
|:---|---:|
| [**Configuration**](Configuration.md) | [**Scenes**](Scenes.md) |
