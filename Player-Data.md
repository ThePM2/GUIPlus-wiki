# Player Data

GUIPlus includes a built-in player data storage system that lets you save, retrieve, and manipulate per-player data fields. This enables features like bank balances, kill counters, quest progress, and any custom data you need.

## How It Works

Player data is stored in `plugins/GUIPlus/playerData.yml`, keyed by player UUID. Each player can have any number of custom fields with string or numeric values.

### Storage Format

```yaml
d290a6cf-1234-5678-90ab-cdef12345678:
  deposited: 1500
  kills: 25
  rank: gold
  custom-note: Hello World
```

## Saving Data

Use the `save-player-info` click event to save data fields:

```yaml
click-events:
  save-player-info:
    save-format: field:value
```

### Format

The `save-format` uses the pattern: `field1:value1,field2:value2`

Multiple fields can be saved at once by separating them with commas.

### Setting a Static Value

```yaml
click-events:
  save-player-info:
    save-format: rank:vip
```

### Math Operations

You can perform arithmetic on existing values. The expression evaluator supports a full range of operations:

**Basic Operators:**

| Operator | Description | Example |
|----------|-------------|---------|
| `+` | Addition | `10+5` = 15 |
| `-` | Subtraction | `10-5` = 5 |
| `*` | Multiplication | `10*5` = 50 |
| `/` | Division | `10/5` = 2 |
| `^` | Exponentiation | `2^3` = 8 |

**Functions:**

| Function | Description | Note |
|----------|-------------|------|
| `sqrt(x)` | Square root | `sqrt(16)` = 4 |
| `sin(x)` | Sine | Input in degrees |
| `cos(x)` | Cosine | Input in degrees |
| `tan(x)` | Tangent | Input in degrees |

Parentheses `()` are supported for grouping: `(2+3)*4` = 20.

```yaml
# Add 100 to the existing 'deposited' value
click-events:
  save-player-info:
    save-format: deposited:%input%+%GUIPlus_player_info_deposited%
```

```yaml
# Increment kills by 1
click-events:
  save-player-info:
    save-format: kills:1+%GUIPlus_player_info_kills%
```

```yaml
# Subtract from balance
click-events:
  save-player-info:
    save-format: deposited:%GUIPlus_player_info_deposited%-%input%
```

### Saving Multiple Fields

```yaml
click-events:
  save-player-info:
    save-format: kills:%GUIPlus_player_info_kills%+1,last-kill-time:now
```

## Retrieving Data

Use the `%GUIPlus_player_info_<field>%` placeholder to display saved data anywhere in your GUIs:

```yaml
item-lore:
  - §7Bank Balance: §a$%GUIPlus_player_info_deposited%
  - §7Total Kills: §c%GUIPlus_player_info_kills%
  - §7Rank: §e%GUIPlus_player_info_rank%
```

If a field has not been set for a player, the placeholder returns the string `null`.

## Full Example: Point Shop

```yaml
id: pointshop
rows: 3
type: chest
title: §5Point Shop
commandAlias: pointshop
scenes:
  '0':
    delay: 0
    items:
      '1':
        slot: 4
        item: PLAYER_HEAD
        amount: 1
        item-name: §d§lYour Points
        item-lore:
          - ''
          - §7You have §d%GUIPlus_player_info_points%§7 points
          - ''
        skullBase64: '%self_base64%'
      '2':
        slot: 11
        item: DIAMOND_SWORD
        amount: 1
        item-name: §cWarrior Kit
        item-lore:
          - ''
          - §7Cost: §d50 points
          - ''
          - §eClick to purchase
        conditions:
          conditional-placeholder:
            conditional_condition: '%GUIPlus_player_info_points%>=50'
        conditionFailMessage: §cYou need at least 50 points!
        click-events:
          save-player-info:
            save-format: points:%GUIPlus_player_info_points%-50
          console_command:
            commands:
              - give %player% diamond_sword 1
          message:
            message: §aPurchased Warrior Kit for 50 points!
      '3':
        slot: 15
        item: GOLDEN_APPLE
        amount: 5
        item-name: §6Healing Bundle
        item-lore:
          - ''
          - §7Cost: §d25 points
          - ''
          - §eClick to purchase
        conditions:
          conditional-placeholder:
            conditional_condition: '%GUIPlus_player_info_points%>=25'
        conditionFailMessage: §cYou need at least 25 points!
        click-events:
          save-player-info:
            save-format: points:%GUIPlus_player_info_points%-25
          console_command:
            commands:
              - give %player% golden_apple 5
          message:
            message: §aPurchased Healing Bundle for 25 points!
```

---

[Previous: Chat Fetcher](Chat-Fetcher) | [Next: Custom Heads & Skulls](Custom-Heads-and-Skulls)
