> 🎨 **CREATING GUIS**

# Conditions

*Choose what players can see, open and execute*

---

Conditions check the player **viewing** the menu. Put them at the level that matches your goal:

| Location | Purpose |
|----------|---------|
| Scene `open-conditions` | Allow entry to a scene |
| Item `showConditions` | Show this item or try its nested fallback |
| Item `conditions` | Allow clicking the displayed item |
| Action `conditions` | Allow that action to execute |
| Chat Fetcher `conditions` | Validate the submitted answer before child actions |

Hiding an item and authorizing its actions are separate settings. A failed click requirement leaves the item visible and displays a failure message. All requirements in one list must pass.

## 📐 Condition Structure

Use the registered ID as the key, or a unique custom key with an explicit `condition`:

```yaml
showConditions:
  has-permission:
    permission: server.vip
conditions:
  minimum-balance:
    condition: has-money
    required-balance: 1000
conditionFailMessage: '&cYou need 1000 coins.'
```

Quote text starting with `&` or `%`. Unknown IDs and unsupported parameters reject the affected definition; requirements are never silently removed. A registered condition whose required provider is unavailable denies at runtime. Types registered only with an integration, such as `player-points`, cannot load while that integration is absent.

## 📋 Quick Reference

| ID | Check | Fields |
|----|-------|--------|
| `has-money` | Vault economy balance | `required-balance` |
| `has-permission` | Permission node | `permission` |
| `has-empty-slots` | Empty player storage slots | `free-slots` |
| `is-integer` | Parseable whole number | `value` |
| `is-double` | Parseable finite decimal | `value` |
| `conditional-placeholder` | Literal or numeric comparison | `conditional_condition` |
| `cooldown` | Per-player timer | `cooldown`, `id` |
| `has-items` | Matching item quantities | `items`, `ignore-durability` |
| `level-required` | XP levels | `level` |
| `xp-required` | Experience points | `xp` |
| `practical-reward-status` | Practical reward availability | `reward`, `state` |
| `player-points` | PlayerPoints balance; available with PlayerPoints | `amount` |

## ⚙ Common Properties

Every condition accepts `inverted: true` to reverse an ordinary true/false result. For example, an inverted `is-integer` check matches text that is not a parseable integer. Invalid or unresolved comparison operands, unavailable required providers and evaluation errors remain denied rather than passing through inversion.

```yaml
conditions:
  has-permission:
    permission: server.blocked
    inverted: true
```

## ✅ All Condition Types

### Has Money
**ID:** `has-money`

Check that the viewer has at least this Vault balance. Requires Vault and an economy provider. `required-balance` accepts a number or supported text substitution.

```yaml
conditions:
  has-money:
    required-balance: 500
```

The old `has-balance` ID remains accepted; new saves use `has-money`.

### Has Permission
**ID:** `has-permission`

```yaml
conditions:
  has-permission:
    permission: server.vip
```

Check the viewer's permission. Targeting a different player with a menu does not change whose permission is checked.

### Empty Slots
**ID:** `has-empty-slots`

Require a minimum of `0`–`36` empty slots in the viewer's player inventory storage area:

```yaml
conditions:
  has-empty-slots:
    free-slots: 5
```

This counts empty slots; it does not calculate whether a particular reward bundle fits into partial stacks. Practical rewards perform their own whole-bundle fit check.

### Integer Validation
**ID:** `is-integer`

Check that text can be parsed as an integer. This does not compare its size.

```yaml
conditions:
  is-integer:
    value: '%input%'
```

For new chat prompts, [Input Rules](Input-Rules.md) let you choose INTEGER and minimum/maximum values directly.

### Double Validation
**ID:** `is-double`

Check that text can be parsed as a finite decimal number. Use `conditional-placeholder` for comparisons, or DECIMAL [Input Rules](Input-Rules.md) for bounded player input.

```yaml
conditions:
  is-double:
    value: '%input%'
```

### Conditional Placeholder
**ID:** `conditional-placeholder`

Compare two values with exactly one supported operator:

| Operator | Meaning |
|----------|---------|
| `=` | Exact string equality |
| `!=` | Exact string inequality |
| `(=)` | Left text contains right text |
| `(!=)` | Left text does not contain right text |
| `<`, `>`, `<=`, `>=` | Finite numeric comparison |

Use the field `conditional_condition`. For string comparisons, spaces are part of the text.

```yaml
conditions:
  progress:
    condition: conditional-placeholder
    conditional_condition: '%GUIPlus_player_info_points%>=50'
```

GUIPlus resolves its own `%GUIPlus_player_info_<field>%` tokens locally for the viewer in this condition. Other plugins' placeholders need PlaceholderAPI and their expansions. Missing fields return `null`; numeric comparisons with missing or unresolved values fail.

`=` compares text, so `1` and `1.0` differ. For numeric equality, use separate `>=` and `<=` conditions with explicit custom IDs.

### Cooldown
**ID:** `cooldown`

```yaml
conditions:
  cooldown:
    cooldown: 30000
    id: example-button
conditionFailMessage: '&cTry again in %cooldown_remaining_seconds% seconds.'
```

`cooldown` is a whole number of **milliseconds**; `30000` means 30 seconds. The editor also accepts suffixes such as `30s`, `1h` and `1d` during setup. Keep a stable `id` to share or retain a timer; when it is omitted, a fallback ID is derived from the condition's configuration path.

A successful cooldown evaluation starts its timer. Merely placing it in a visibility list can therefore consume it while the menu is displayed. Use it in click/action requirements, and place it after read-only checks that should run first.

The remaining-time token reports the first failed cooldown for that viewer, rounded up to seconds. Reading it does not start or extend the timer. Default denials use `default_fail_cooldown` in `messages.yml`; `cooldown_remaining_suffix` adds the time to older messages that lack the token. Set that suffix to `''` to disable the automatic addition.

[Button States](Button-States.md) visibility and [Interaction Flows](Interaction-Flows.md) requirements accept read-only guards, so they exclude cooldown. Put cooldown on the click or confirmed action instead.

### Has Items
**ID:** `has-items`

Check for the whole configured quantity of matching items without removing them. Use the in-game editor to copy the real stacks into `items`, preserving their metadata.

`ignore-durability: true` ignores item damage when matching. The [Take Item action](Click-Events.md#take-item) removes items and uses the different spelling `ignoreDurability`.

### Level
**ID:** `level-required`

```yaml
conditions:
  level-required:
    level: 30
```

Require at least 30 XP levels without taking them. The action with this same ID removes levels; the enclosing `conditions` or `click-events` map determines which one you are authoring.

### XP
**ID:** `xp-required`

```yaml
conditions:
  xp-required:
    xp: 1000
```

Require experience points rather than XP levels.

### Practical Reward Status
**ID:** `practical-reward-status`

Check a canonical reward's current state without claiming it:

```yaml
showConditions:
  practical-reward-status:
    reward: daily-1
    state: READY
```

States are `LOCKED`, `READY`, `OWNED` and `BLOCKED`. Use the [Practical Reward action](Click-Events.md#practical-reward) to claim, and see [Ready-to-Use Rewards](Ready-to-Use-Rewards.md) for the stable reward IDs.

### Player Points
**ID:** `player-points`

Registered when PlayerPoints is available. Check the viewer's balance without removing it:

```yaml
conditions:
  player-points:
    amount: 500
```

## 👁 Visibility and Click Requirements

This item appears only for VIP viewers and requires 50 points when clicked:

```yaml
'1':
  slot: 13
  item: DIAMOND_SWORD
  item-name: '&6VIP Information'
  showConditions:
    has-permission:
      permission: server.vip
  conditions:
    enough-points:
      condition: conditional-placeholder
      conditional_condition: '%GUIPlus_player_info_points%>=50'
  conditionFailMessage: '&cYou need 50 points.'
  click-events:
    message:
      message: '&aYou meet the requirements.'
```

## 📊 Nested Items and Sorting Groups

`nestedItems` supplies alternative appearances in the same slot. GUIPlus checks the parent first, then its nested alternatives, choosing the first item whose visibility requirements pass. See [Creating GUIs](Creating-GUIs.md).

`sorting-group` has a different purpose: items with the same group name use that group's existing slots in order, compacting visible entries into the first available group slots. It is a normal saved YAML field and can also be set in the item editor. It does not require all items to have the same slot.

For a reusable Locked / Ready / Owned button, use [Button States](Button-States.md). Explicitly linked [Shared Components](Shared-Components.md) can supply the same requirements and actions to several menus.

---

| ← Previous | Next → |
|:---|---:|
| [**Click Events**](Click-Events.md) | [**Chat Fetcher**](Chat-Fetcher.md) |
