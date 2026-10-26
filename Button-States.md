> 🎨 **CREATING GUIS**

# Button States

*Show Locked, Ready and Owned versions of one menu button*

---

A state button keeps three appearances and their actions together. Use it for unlocks, reward claims or progression cards that change as a player's requirements change.

## 🛠 Create a State Button

1. Open `/gui`, edit your GUI and select an item.
2. Choose **Button States**. If you use an older customized editor layout, **Shift-right-click Nested Items** opens the same screen.
3. Edit the **Locked**, **Ready** and **Owned** appearances, click actions and requirements.
4. Add **Shared parameters** for text or values reused by several states.
5. Preview the appearance, then choose **Apply state button**.
6. Return to the GUI settings and **Save Gui**.

The editor works on a draft. Cancel or closing it discards that draft. Preview displays the authored appearance; it does not evaluate a player's requirements or run actions.

> **Try an example:** `/gui examples progression my_progress` installs an independent progression menu with an editable state button and a way to set sample progress. Example installation requires `guiplus.gui.examples` and the configured `/gui` root permission. It never replaces an existing menu ID.

## ✅ How a State Is Selected

| State | When it appears |
|---|---|
| **Owned** | It has at least one visibility requirement and all its requirements pass. Checked first. |
| **Ready** | Owned did not match, and Ready has at least one visibility requirement that all pass. |
| **Locked** | Neither Owned nor Ready matched. This is the fallback and has no visibility requirements. |

Visibility requirements are stored in `showConditions`. Click requirements are stored in `conditions`. The original item's common click requirements still apply once, in addition to the selected state's click requirements.

State visibility supports the built-in read-only permission, balance, item, placeholder, number, PlayerPoints, level, XP, empty-slot and practical-reward checks. Integration-backed checks still need their usual dependencies. Put a consuming **Cooldown** condition in click requirements, where starting its timer belongs.

The button refreshes inside the open inventory when its state changes. A click on an obsolete appearance is denied and refreshed so the player can choose the newly displayed action.

> **Ownership:** Configure an Owned requirement that reads actual progress or a permission. Sending a command does not automatically prove that its reward succeeded.

## 🔗 Shared Parameters

Reference a parameter with `%state:key%`, such as `%state:label%`. Supported text fields include names, lore, messages and string-based action or condition values. For example, a shared threshold can appear both in lore and in a `conditional-placeholder` expression.

Use up to **32 parameters**. Names use **1–32 lowercase letters, digits, `_` or `-`**, starting with a letter. Values may be text, numbers or booleans and are limited to **256 characters**, without control characters. Parameters cannot reference other state parameters.

Keep material, item amount, timing and strict numeric fields as their expected literal values. Missing parameters or unsupported bindings reject the definition. Parameter replacement does not rewrite opaque item metadata.

## 📐 Example: A Daily Reward Button

Place this item fragment under `scenes.<scene-id>.items` in an existing GUI with at least three rows. It uses the existing `daily-1` reward, so it shares progress with `/rewards daily`. The default prize is 8 bread; changing the actual reward catalog changes that prize.

```yaml
'13':
  slot: 13
  item: BREAD
  amount: 1
  item-name: '&eDaily reward'
  state-button:
    parameters:
      label: 'Daily reward'
    states:
      locked:
        item: GRAY_DYE
        amount: 1
        item-name: '&7%state:label% &cLocked'
        item-lore:
          - '&7Next claim: %GUIPlus_reward_daily_remaining%'
      ready:
        item: BREAD
        amount: 1
        item-name: '&a%state:label% &aReady'
        item-lore:
          - '%GUIPlus_reward_bundle_daily-1%'
          - '&eClick to claim.'
        showConditions:
          '0':
            condition: practical-reward-status
            reward: 'daily-1'
            state: 'READY'
        click-events:
          '0':
            type: practical-reward
            reward: 'daily-1'
      owned:
        item: EMERALD
        amount: 1
        item-name: '&6%state:label% &6Owned'
        showConditions:
          '0':
            condition: practical-reward-status
            reward: 'daily-1'
            state: 'OWNED'
```

The `state-button` mapping requires exactly `locked`, `ready` and `owned`. Keep a visible, non-AIR base item on the outer button. Disable states before removing that base item. State definitions cannot contain another state button.

See [Ready-to-Use Rewards](Ready-to-Use-Rewards.md) for claim rules, actual prize editing and saved progress.

---

| ← Previous | Next → |
|:---|---:|
| [**Conditions**](Conditions.md) | [**Interaction Flows**](Interaction-Flows.md) |
