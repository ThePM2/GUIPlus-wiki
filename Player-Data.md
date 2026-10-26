> 💬 **INTERACTIVE**

# Player Data

*Save named values for each player's UUID*

---

GUIPlus stores custom fields in `plugins/GUIPlus/playerData.yml`, under the player's UUID. Use them for progression, choices, counters and other menu data. Menu copies using the same field names read the same per-player values.

## 📐 Stored Fields

```yaml
'd290a6cf-1234-5678-90ab-cdef12345678':
  points: '100'
  rank: 'gold'
  custom-note: 'Hello World'
```

The UUID identifies the player; the field name identifies the value. Prefer clear names such as `quest_points` or `myserver_rank`. GUI IDs, scene IDs and item IDs are separate from player-data fields. Renaming a menu does not rename its player data.

## 💾 Saving Data

Add the `save-player-info` action. `save-format` contains `field:value` pairs separated by commas:

```yaml
click-events:
  save-player-info:
    save-format: 'rank:literal:vip,points:number:100'
```

The action writes to the actual target, normally the viewer. Multiple fields in one action are saved together; a failed update does not publish a partial set.

### Explicit Value Formats

| Prefix | Purpose | Example |
|--------|---------|---------|
| `literal:` | Keep text and punctuation | `note:literal:part-time` |
| `number:` | Validate and normalize a number | `points:number:100.00` |
| `calc:` | Evaluate arithmetic | `total:calc:2+3` |

```yaml
click-events:
  save-player-info:
    save-format: 'note:literal:part-time,date:literal:2026-10-26,total:calc:2+3'
```

Without a prefix, ordinary text remains text: `2+3` is not automatically a calculation. Older arithmetic templates containing placeholders still work, but explicit prefixes make new definitions clearer. Player-entered text does not become arithmetic merely because it contains `+` or `-`.

Commas separate entries and cannot be escaped inside a value. A colon after the field name stays in that value, making the format prefixes possible.

### Calculations

`calc:` supports `+`, `-`, `*`, `/`, `^`, parentheses, and `sqrt()`, `sin()`, `cos()` and `tan()`. Trigonometric inputs use degrees. Calculations must produce a finite number.

To initialize and then change a counter:

```yaml
click-events:
  initialize:
    type: save-player-info
    save-format: 'points:number:0'
```

```yaml
click-events:
  increment:
    type: save-player-info
    save-format: 'points:calc:%GUIPlus_player_info_points%+1'
```

Reading an existing field through the PlaceholderAPI token in this action requires PlaceholderAPI. Initialize the field first: an unset value is `null`, not zero. Unresolved or invalid calculations stop rather than saving a corrupted result.

Within one `save-player-info` action, later entries can reference fields already set by that same action:

```yaml
save-format: 'base:number:10,total:calc:%GUIPlus_player_info_base%+5'
```

## 📖 Reading Data

With PlaceholderAPI installed, use `%GUIPlus_player_info_<field>%` in ordinary item names and lore:

```yaml
item-name: '&6Your Profile'
item-lore:
  - '&7Points: &e%GUIPlus_player_info_points%'
  - '&7Rank: &a%GUIPlus_player_info_rank%'
```

An unset field returns the text `null`. GUIPlus also resolves its own player-info tokens locally in `conditional-placeholder` conditions and [Button States](Button-States.md) display text, so those uses do not require PlaceholderAPI.

Conditions inspect the viewer's fields, even if the menu has a different action target. See [PlaceholderAPI Placeholders](PlaceholderAPI-Placeholders.md) for each token's context.

## 🔒 Progression Example

This item is visible to everyone, but its click requires the viewer to have at least 50 saved points. It only sends information; it does not spend the points.

```yaml
'1':
  slot: 13
  item: DIAMOND
  item-name: '&bProgress Check'
  conditions:
    enough-points:
      condition: conditional-placeholder
      conditional_condition: '%GUIPlus_player_info_points%>=50'
  conditionFailMessage: '&cYou need at least 50 points.'
  click-events:
    message:
      message: '&aYou meet the progression requirement.'
```

Use `showConditions` instead of `conditions` when the goal is to hide an item. For Locked / Ready / Owned appearances, start with [Button States](Button-States.md) or `/gui examples progression my_progress`.

## 🏦 Bank Balances

For money moving between Vault and a recorded field, use `bank-transfer`, rather than composing independent `money-remove`, `save-player-info` and `money-give` actions:

```yaml
click-events:
  bank-transfer:
    direction: WITHDRAW
    field: deposited
    amount: '%input%'
```

See [Chat Fetcher](Chat-Fetcher.md#bank-deposit-example) for a bounded amount prompt. Vault and a working economy provider are required.

An interrupted or uncertain provider payment remains blocked for review. `/guiplus-transactions list` and `show <id>` inspect operations. After checking provider records, `confirm-success <id> <evidence>` or `confirm-failure <id> <evidence>` reconciles the local ledger without repeating the payment. Use one evidence token, such as `ticket-123`.

`/guiplus-transactions refunds` lists unresolved refunds. `resolve-refund <id> <evidence>` records a verified manual recovery and sends no items or money. All these commands require `guiplus.transactions`.

Generic command lists do not make a cross-plugin purchase atomic. A command being dispatched also does not prove a rank or reward was delivered.

## 🎁 Practical Reward Progress

[Ready-to-Use Rewards](Ready-to-Use-Rewards.md) uses stable reward IDs and the `guiplus_practical_v1` namespace under each player's UUID. Daily waits, completed playtime milestones and the one starter choice survive menu copies and layout edits.

Edit actual prizes through `/gui practical edit`. Avoid changing internal reward progress by hand. Use the documented receipt commands when a claim requires reconciliation.

## 🗂 Backups and Manual Changes

Back up `playerData.yml` with normal Minecraft player data, the practical reward catalog and bank/refund journals. When repairing custom fields manually, stop the server first, or run `/gui reload` immediately after saving the file so the live copy cannot overwrite the edit. Reload also closes existing editing/input sessions.

---

| ← Previous | Next → |
|:---|---:|
| [**Chat Fetcher**](Chat-Fetcher.md) | [**Custom Heads & Skulls**](Custom-Heads-and-Skulls.md) |
