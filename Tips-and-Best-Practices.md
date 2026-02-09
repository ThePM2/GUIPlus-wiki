> 💡 **RESOURCES**

# Tips & Best Practices

*Performance optimization, organization patterns, and common pitfalls*

---

## 📁 File Organization

### Naming Conventions

Use consistent, descriptive names for your GUI files and IDs:

| Pattern | Example | Use Case |
|---------|---------|----------|
| Feature name | `shop.yml`, `bank.yml` | Simple, single-purpose GUIs |
| Category prefix | `admin-ban.yml`, `admin-kick.yml` | Related GUIs grouped by category |
| Lowercase with dashes | `server-selector.yml` | Multi-word names |

Avoid spaces, special characters, and uppercase in file names and GUI IDs. The ID is case-sensitive when used in commands.

### Folder Structure

Keep your `CustomGuis/` folder organized. While GUIPlus reads all `.yml` files from this folder, a clean structure helps you manage large GUI collections:

```
plugins/GUIPlus/CustomGuis/
├── shop.yml           # Main shop
├── bank.yml           # Banking system
├── warps.yml          # Warp menu
├── profile.yml        # Player profile
├── adminpanel.yml     # Admin tools
├── rules.yml          # Server rules
└── daily.yml          # Daily rewards
```

> **Tip:** GUIPlus does not support subfolders inside `CustomGuis/`. All GUI files must be in the root of the folder.

---

## ⚡ Performance

### Minimize Placeholder Usage in Lore

PlaceholderAPI placeholders in item lore are re-evaluated every second (20 ticks). Having many placeholders across many items can increase server load:

**Avoid:**
```yaml
# 10 items each with 5 placeholder lore lines = 50 placeholder evaluations per second
item-lore:
  - §7Stat 1: %statistic_mine_block_diamond_ore%
  - §7Stat 2: %statistic_mine_block_iron_ore%
  - §7Stat 3: %statistic_mine_block_gold_ore%
  - §7Stat 4: %statistic_mine_block_coal_ore%
  - §7Stat 5: %statistic_mine_block_emerald_ore%
```

**Better:** Consolidate data into fewer items or use static text where values don't change frequently:

```yaml
item-lore:
  - §7Total Mined: %statistic_mine_block%
```

### Keep GUIs Focused

Rather than building one massive GUI with many scenes, create separate GUIs for different purposes and link them with commands:

```yaml
# In your main menu, link to sub-menus
click-events:
  command:
    commands:
      - gui open shop
```

This keeps each GUI file manageable and reduces the number of items loaded at once.

### Use Conditions Efficiently

Conditions are evaluated when the GUI opens and when items refresh. Simpler conditions are faster:

- `has-permission` — Very fast (permission lookup)
- `has-money` — Fast (single Vault call)
- `conditional-placeholder` — Speed depends on the placeholder being evaluated
- `cooldown` — Fast (timestamp comparison)

---

## 📝 YAML Tips

### Quoting Strings

Always quote strings that start with special YAML characters (`%`, `&`, `*`, `!`, `{`, `[`, etc.):

```yaml
# These need quotes because % is a special character indicator in some parsers
conditionFailMessage: '§cYou need at least $500!'
conditional_condition: '%player_level%>=10'
```

Single quotes (`'`) are safest for Minecraft formatting codes since `§` and `&` don't need escaping.

### Empty Lore Lines

Use `''` (empty single-quoted string) for blank lines in lore:

```yaml
item-lore:
  - ''
  - §7This has spacing above and below
  - ''
```

### Number vs String

Some fields accept both numbers and placeholder strings. When using a placeholder, quote it:

```yaml
# Static number — no quotes needed
money-remove:
  amount: 500

# Dynamic placeholder — quotes needed
money-remove:
  amount: '%input%'
```

### Indentation

YAML uses spaces, not tabs. Use 2-space indentation consistently:

```yaml
scenes:
  '0':
    delay: 0
    items:
      '1':
        slot: 0
        item: DIAMOND
```

> **Tip:** Use a YAML validator (search "YAML lint" online) to catch indentation and syntax errors before reloading.

---

## 🔁 Common Patterns

### Glass Pane Borders

Fill empty slots with glass panes to create clean borders:

```yaml
# Repeat for each border slot
'100':
  slot: 0
  item: BLACK_STAINED_GLASS_PANE
  amount: 1
  item-name: ' '
'101':
  slot: 1
  item: BLACK_STAINED_GLASS_PANE
  amount: 1
  item-name: ' '
# ... continue for all border slots
```

> **Tip:** Use the in-game editor to place border items quickly rather than writing YAML by hand.

### Back Button Pattern

Add a consistent "back" button that returns to the previous GUI:

```yaml
'99':
  slot: 45
  item: ARROW
  amount: 1
  item-name: §7← Back
  click-events:
    back: {}
    sound-click-event:
      sound: UI_BUTTON_CLICK
```

### Close Button Pattern

```yaml
'100':
  slot: 49
  item: BARRIER
  amount: 1
  item-name: §c§lClose
  click-events:
    close-inventory: {}
```

### Sound Feedback

Always add sound feedback to click events. It gives players clear confirmation that their action worked:

```yaml
click-events:
  sound-click-event:
    sound: UI_BUTTON_CLICK    # Neutral click
  # or
  sound-click-event:
    sound: ENTITY_PLAYER_LEVELUP  # Success/purchase
  # or
  sound-click-event:
    sound: ENTITY_VILLAGER_NO     # Denied/error
```

### Combining Left/Right Click Actions

Use `clickType` to give items dual functionality:

```yaml
click-events:
  buy-action:
    message: §aPurchased!
    clickType: LEFT
  info-action:
    message: §7This item costs $500
    clickType: RIGHT
```

Add instructions in the lore so players know:
```yaml
item-lore:
  - ''
  - §eLeft-click to buy
  - §7Right-click for info
```

---

## ⚠ Common Pitfalls

### Duplicate YAML Keys

YAML maps cannot have duplicate keys. If you need two click events of the same type, use unique keys:

**Wrong:**
```yaml
click-events:
  message:
    message: §aFirst message
  message:              # This overwrites the first one!
    message: §bSecond message
```

**Correct:**
```yaml
click-events:
  first-message:
    message: §aFirst message
  second-message:
    message: §bSecond message
```

### Forgetting to Quote Placeholders in Conditions

Placeholder values in conditions must be quoted:

**Wrong:**
```yaml
conditional_condition: %player_level%>=10
```

**Correct:**
```yaml
conditional_condition: '%player_level%>=10'
```

### Using = for Numeric Comparison

The `=` operator in `conditional-placeholder` does **string** comparison, not numeric. `10` does not string-equal `10.0`:

**Wrong (for numeric equality):**
```yaml
conditional_condition: '%vault_eco_balance%=1000'
```

**Correct:**
```yaml
# Use >= and <= together for numeric equality
conditional_condition: '%vault_eco_balance%>=1000'
```

### Cooldown Values in Milliseconds

Cooldown durations are in **milliseconds**, not seconds:

| Duration | Milliseconds |
|----------|-------------|
| 1 second | `1000` |
| 30 seconds | `30000` |
| 5 minutes | `300000` |
| 1 hour | `3600000` |
| 24 hours | `86400000` |

### Material Names

Use the Bukkit material name, not the Minecraft display name:

| Display Name | Correct Material |
|-------------|-----------------|
| Crafting Table | `CRAFTING_TABLE` |
| Ender Chest | `ENDER_CHEST` |
| Enchanting Table | `ENCHANTING_TABLE` |
| Jack o'Lantern | `JACK_O_LANTERN` |

Check the [Bukkit Material list](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/Material.html) for exact names.

---

## 🔒 Security Considerations

### The `setOp` Flag

The `command` click event has a `setOp: true` option that temporarily gives the player OP to execute a command. **Avoid using this.** Instead, use `console_command` to run privileged commands from the console:

**Avoid:**
```yaml
click-events:
  command:
    commands:
      - give %player% diamond 1
    setOp: true
```

**Prefer:**
```yaml
click-events:
  console_command:
    commands:
      - give %player% diamond 1
```

### Input Validation

When using chat fetcher for player input, always validate:

1. **Type check** — Use `is-integer` or `is-double` to ensure the input is a number
2. **Range check** — Use `conditional-placeholder` to check bounds (e.g., `%input%>0`)
3. **Economy check** — Use `has-money` to verify funds before deducting

```yaml
conditions:
  is-integer:
    value: '%input%'
  conditional-placeholder:
    conditional_condition: '%input%>0'
  has-money:
    required-balance: '%input%'
```

---

| ← Previous | Next → |
|:---|---:|
| [**Premade Configurations**](Premade-Configurations) | [**Home**](Home) |
