> 🎨 **CREATING GUIS**

# Conditions

*Control item visibility and click event execution*

---

Conditions control whether an item is shown to a player and whether click events are allowed to execute. If a condition fails, the item is hidden (or replaced by a nested fallback item), and the `conditionFailMessage` is shown.

## 📐 Condition Structure

Conditions are defined inside an item's `conditions` map:

```yaml
conditions:
  has-permission:
    permission: admin.panel
  has-money:
    required-balance: 1000
conditionFailMessage: &cYou don't meet the requirements!
```

## Common Properties

Every condition supports:

| Property | Type | Description |
|----------|------|-------------|
| `inverted` | Boolean | When `true`, the condition result is negated (passes when it would normally fail) |

## ✅ All Condition Types

### Has Money
**ID:** `has-money`

Checks if the player has at least the specified balance. Requires Vault.

```yaml
conditions:
  has-money:
    required-balance: 500
```

The `required-balance` field supports PlaceholderAPI placeholders.

---

### Has Permission
**ID:** `has-permission`

Checks if the player has a specific permission node.

```yaml
conditions:
  has-permission:
    permission: myserver.vip
```

To check if a player does **not** have a permission:

```yaml
conditions:
  has-permission:
    permission: myserver.banned
    inverted: true
```

---

### Empty Slots
**ID:** `has-empty-slots`

Checks if the player's inventory has a minimum number of empty slots.

```yaml
conditions:
  has-empty-slots:
    free-slots: 5
```

The YAML field is `free-slots` (not `slots`).

---

### Integer Validation
**ID:** `is-integer`

Validates that a value is a valid integer. Useful for validating chat fetcher input to ensure the player typed a whole number.

```yaml
conditions:
  is-integer:
    value: '%input%'
```

> **Note:** This condition only checks if the value can be parsed as an integer. It does not support comparison operators.

---

### Double Validation
**ID:** `is-double`

Validates that a value is a valid decimal number. Useful for validating chat fetcher input that should accept decimals.

```yaml
conditions:
  is-double:
    value: '%input%'
```

> **Note:** This condition only checks if the value can be parsed as a decimal number. It does not support comparison operators. Use `conditional-placeholder` for numeric comparisons.

---

### Conditional Placeholder
**ID:** `conditional-placeholder`

Compares two values using PlaceholderAPI placeholders. This is the most versatile condition type and the one to use for numeric comparisons.

**Operators:**

| Operator | Description |
|----------|-------------|
| `=` | String equals (exact match) |
| `!=` | String not equals |
| `<` | Numeric less than |
| `>` | Numeric greater than |
| `<=` | Numeric less than or equal |
| `>=` | Numeric greater than or equal |
| `(=)` | String contains check |
| `(!=)` | String not-contains check |

The `=` and `!=` operators perform **string** comparison, not numeric. For numeric equality, use `>=` and `<=` together. The `(=)` operator checks if the left side **contains** the right side; `(!=)` checks that it does **not contain** it.

The YAML field is `conditional_condition` (not `condition`):

```yaml
conditions:
  conditional-placeholder:
    conditional_condition: '%player_level%>=10'
```

Example checking if the player can afford a withdrawal:
```yaml
conditions:
  conditional-placeholder:
    conditional_condition: '%GUIPlus_player_info_deposited%>=%input%'
```

---

### Cooldown
**ID:** `cooldown`

Enforces a time-based cooldown before the condition passes again.

```yaml
conditions:
  cooldown:
    cooldown: 30000
    id: my-cooldown
```

| Property | Type | Description |
|----------|------|-------------|
| `cooldown` | Long | Cooldown duration in **milliseconds** |
| `id` | String | A unique identifier for this cooldown |

> **Note:** The value is in milliseconds. Use `1000` for 1 second, `30000` for 30 seconds, `3600000` for 1 hour. The `id` field is required to track cooldown state per player. Time unit suffixes (`s`, `ms`, `h`, `d`) are only supported in the in-game editor setup, not in the YAML file directly.

---

### Has Items
**ID:** `has-items`

Checks if the player's inventory contains specific items. Items are stored as serialized Bukkit ItemStack objects.

> **Tip:** This is best configured through the in-game editor. The editor lets you select which items to check for.

```yaml
conditions:
  has-items:
    items:
      - ==: org.bukkit.inventory.ItemStack
        v: 3700
        type: DIAMOND
        amount: 10
    ignore-durability: false
```

| Property | Type | Description |
|----------|------|-------------|
| `items` | List | List of serialized Bukkit ItemStack objects to check for |
| `ignore-durability` | Boolean | When `true`, ignores item durability when matching |

---

### Level
**ID:** `level-required`

Checks if the player has at least the specified XP level.

```yaml
conditions:
  level-required:
    level: 30
```

---

### XP
**ID:** `xp-required`

Checks if the player has at least the specified total experience points.

```yaml
conditions:
  xp-required:
    xp: 1000
```

---

### Player Points
**ID:** `player-points`

Checks the player's PlayerPoints balance. Requires the PlayerPoints plugin.

```yaml
conditions:
  player-points:
    amount: 500
```

## 🔗 Using Conditions with Click Events

Conditions can be applied at two levels:

### 1. Item-Level Conditions

Controls whether the item is **visible** to the player:

```yaml
'1':
  slot: 13
  item: DIAMOND_SWORD
  item-name: &6VIP Sword
  conditions:
    has-permission:
      permission: server.vip
  conditionFailMessage: &cYou need VIP to see this!
```

### 2. Chat Fetcher Conditions

Validates input received from the chat fetcher:

```yaml
click-events:
  chat-fetcher:
    message: &eEnter an amount:
    conditionFailMessage: &cInvalid amount!
    conditions:
      is-integer:
        value: '%input%'
      conditional-placeholder:
        conditional_condition: '%input%>0'
    click-events:
      money-remove:
        amount: '%input%'
```

> **Tip:** For numeric comparisons in chat fetcher validation, use `conditional-placeholder` with the `conditional_condition` field. The `is-integer` and `is-double` conditions only validate format, not value ranges.

## 🔄 Inverted Conditions

Any condition can be inverted to check for the **opposite**:

```yaml
conditions:
  has-permission:
    permission: server.banned
    inverted: true
```

This passes when the player does **not** have the `server.banned` permission.

## 📊 Sorting Groups

Sorting groups allow items to serve as fallbacks for each other. If the primary item's condition fails, the next item in the group is checked — the first item whose conditions pass is displayed. Items in the same sorting group must share the same `slot` number.

> **Note:** Sorting groups are configured through the in-game editor. The `sorting-group` value is stored in the item's compressed data and is not a hand-writable YAML field. Use the in-game editor to assign items to sorting groups.

**Example behavior:** If you have a VIP item and a default item in the same sorting group on slot 13, VIP players see the VIP item while non-VIP players see the default item.

---

| ← Previous | Next → |
|:---|---:|
| [**Click Events**](Click-Events) | [**Chat Fetcher**](Chat-Fetcher) |
