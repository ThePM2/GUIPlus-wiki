> 📖 **TUTORIALS & RESOURCES**

# Tips & Best Practices

*Keep menus easy to edit, test and use.*

---

## 📁 Organize Menus by Purpose

Use clear IDs such as `server_menu`, `staff_panel` and `travel`. Give each menu a different ID and file name. Use [Scenes](Scenes.md) for pages within a menu; use [Shared Components](Shared-Components.md) for a button reused by several menus.

An ordinary copy is independent. A linked component is shared. Choose deliberately instead of expecting copied buttons to update each other.

## 📝 Write YAML the Plugin Can Read

### Quote Text

```yaml
title: '&8Server Menu'
item-name: '&bInformation'
item-lore:
  - '&7Balance: %vault_eco_balance%'
  - ''
```

Use spaces, not tabs. Quote placeholders, color codes and values containing `: `. Keep lists as lists and numeric settings as numbers unless their field explicitly accepts a placeholder string.

### Give Repeated Actions Unique Labels

Duplicate YAML keys are rejected. Use a different label and an explicit action `type`:

```yaml
click-events:
  first_message:
    type: message
    message: '&aWelcome!'
  second_message:
    type: message
    message: '&7Read /rules before playing.'
```

The same applies to conditions: a custom label needs `condition: has-permission`, for example. Registered IDs such as `message` can still be used directly.

### Keep Encoded Item Data Together

The editor may save a serialized item, its envelope and raw item data alongside simple appearance fields. Use the editor to change a copied custom item instead of changing one part of those encoded representations by hand.

## ✅ Separate Appearance from Access

- Menu `permission` and scene `open-conditions` control entry.
- Item `showConditions` controls which appearance is shown.
- Item/action `conditions` guards the click.

Hiding a paid or privileged item is not a replacement for checking its action requirements. Test permission-based menus with a non-OP player as well as an administrator.

For `conditional-placeholder`, `=` compares strings. To require numeric equality, use two separately named checks, for example:

```yaml
conditions:
  at_least:
    condition: conditional-placeholder
    conditional_condition: '%player_level%>=10'
  at_most:
    condition: conditional-placeholder
    conditional_condition: '%player_level%<=10'
```

This example needs PlaceholderAPI's Player expansion. A single `>=10` check alone means at least ten, not exactly ten.

## ⏱ Keep Units Straight

| Setting | Unit |
|---------|------|
| `cooldown` condition | Milliseconds: `60000` is one minute. |
| Scene/lore `delay`, action `executionDelay` | Server ticks: `20` is normally one second. |
| `gui-settings.update-delay` | Ticks; default `20`. |
| `chat-fetcher-exit-message-cooldown` | Seconds. |
| Practical reward editor time entries | Whole minutes. |

Put a cooldown in click requirements. Evaluating it can start its timer; it is not a read-only display requirement for a state button or a flow confirmation guard.

## 💬 Validate Input Before Using It

For a quantity, use [Input Rules](Input-Rules.md) with `INTEGER` and a useful range such as 1–64. Add the relevant balance or permission check too. Use enum choices for a small fixed set instead of accepting arbitrary command text.

Place effects that need review inside an [Interaction Flow](Interaction-Flows.md)'s Confirmed actions. Values such as `%flow_amount%` belong to that flow; do not expect them in unrelated parent actions.

## 🧭 Common Buttons

These fragments belong under an item's `click-events`:

```yaml
back: {}
```

```yaml
close-inventory: {}
```

```yaml
sound-click-event:
  sound: UI_BUTTON_CLICK
  volume: 1.0
  pitch: 1.0
```

`back` opens the player's previous GUI; `previous-scene-click` stays within the current GUI. They are different kinds of navigation.

## ⚡ Keep Dynamic Menus Focused

Use placeholders where the changing value helps the player. Static slots are skipped by the dynamic updater. `gui-settings.update-delay` changes refresh frequency; animated lore has its own page delays.

Use `/gui diagnostics` to inspect session counts and measurements when investigating performance. Start with one menu and one player so you can identify which item or integration causes the problem.

For manual pages, set `disable-scene-animation: true`. For an animation, choose delays long enough to read. See [Animations](Animations.md).

## 🔒 Commands and Integrations

Keep console commands under administrator control. Use player commands when the other plugin should apply that player's normal permission and cooldown checks. Avoid granting temporary OP just to run an ordinary menu action.

A list of external commands is not one atomic purchase. Use an integration that confirms the result when you need that guarantee. The included practical rewards and bank-transfer action have their own saved claim/transfer handling; they do not turn every external command into the same system.

## 💾 Save and Test Changes

Save or cancel an editor draft before changing YAML. Reload and reopen the menu afterwards. Keep a matching JAR and data backup before a version update, and check actual prizes separately from their decorative menu items.

Try the complete route as an ordinary player: open the menu, meet or fail its requirements, go Back, cancel a prompt and finish a confirmation. See [Updating & Backups](Updating-and-Backups.md) and [Tutorials](Tutorials.md).

---

| ← Previous | Next → |
|:---|---:|
| [**Premade Configurations**](Premade-Configurations.md) | [**FAQ & Troubleshooting**](FAQ-and-Troubleshooting.md) |
