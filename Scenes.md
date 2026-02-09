> 🎨 **CREATING GUIS**

# Scenes

*Create multi-page GUIs with independent layouts*

---

Scenes allow you to create multi-page GUIs within a single menu. Each scene has its own independent item layout and can be navigated using click events.

> **Tip:** Scenes are zero-indexed. The first scene is `'0'`, the second is `'1'`, and so on. Each scene has completely independent items and layout.

## 📐 Scene Structure

Scenes are defined inside the `scenes` map of a GUI file. Scene IDs are zero-indexed strings:

```yaml
scenes:
  '0':    # First scene (page 1)
    delay: 0
    items:
      '1':
        slot: 13
        item: DIAMOND
        item-name: §bPage 1
        click-events:
          next-scene-click: {}
  '1':    # Second scene (page 2)
    delay: 0
    items:
      '1':
        slot: 13
        item: EMERALD
        item-name: §aPage 2
        click-events:
          previous-scene-click: {}
```

## 🏷 Scene Properties

| Property | Type | Description |
|----------|------|-------------|
| `delay` | Integer | Delay in ticks before this scene opens (0 = instant) |
| `items` | Map | The items displayed in this scene |
| `open-events` | Map | Click events that execute when this scene opens |
| `open-conditions` | Map | Conditions that must be met for open-events to execute |

## 🧭 Navigation Click Events

### Next Scene
Moves to the next scene in the sequence:

```yaml
click-events:
  next-scene-click: {}
```

### Previous Scene
Moves to the previous scene:

```yaml
click-events:
  previous-scene-click: {}
```

## 📂 Scene Open Events

Scenes support two ways to run events when they open:

### Scene-Level Open Events

Define `open-events` directly on the scene to run click events when the scene opens. These are independent of any items:

```yaml
scenes:
  '0':
    delay: 0
    open-events:
      welcome-sound:
        sound: BLOCK_CHEST_OPEN
      welcome-message:
        message: §7Welcome to the shop!
    open-conditions:
      first-visit:
        permission: shop.welcome
    items:
      '1':
        slot: 0
        item: DIAMOND
```

The `open-conditions` control whether the scene's `open-events` execute. If any open-condition fails, the open-events are skipped (but the scene still opens and items are displayed normally).

### Item-Level Open Events (Editor)

Through the in-game editor, individual click events on items can be marked with `openEvent: true` to run when the scene opens instead of on click. The `glow-at-item` click event does this automatically.

## ⏱ Scene Delays

Add a delay before a scene opens (in ticks, 20 ticks = 1 second):

```yaml
scenes:
  '0':
    delay: 0     # Opens immediately
    items: ...
  '1':
    delay: 20    # Opens after 1 second
    items: ...
```

## 🛒 Example: Multi-Page Shop

```yaml
id: shop
rows: 6
type: chest
title: §6Server Shop
commandAlias: shop
scenes:
  '0':
    delay: 0
    items:
      '1':
        slot: 0
        item: DIAMOND_SWORD
        amount: 1
        item-name: §cWeapons
        item-lore:
          - §7Price: §a$500
          - ''
          - §eClick to buy!
        click-events:
          money-remove:
            amount: 500
          console_command:
            commands:
              - give %player% diamond_sword 1
          message:
            message: §aPurchased!
        conditions:
          has-money:
            required-balance: 500
        conditionFailMessage: §cNot enough money!
      # ... more items for page 1 ...
      '50':
        slot: 53
        item: ARROW
        amount: 1
        item-name: §eNext Page →
        item-lore:
          - §7Click to go to page 2
        click-events:
          next-scene-click: {}
  '1':
    delay: 0
    items:
      '1':
        slot: 0
        item: GOLDEN_APPLE
        amount: 1
        item-name: §6Golden Apple
        item-lore:
          - §7Price: §a$100
        click-events:
          money-remove:
            amount: 100
          console_command:
            commands:
              - give %player% golden_apple 1
          message:
            message: §aPurchased!
        conditions:
          has-money:
            required-balance: 100
        conditionFailMessage: §cNot enough money!
      # ... more items for page 2 ...
      '50':
        slot: 45
        item: ARROW
        amount: 1
        item-name: §e← Previous Page
        item-lore:
          - §7Click to go back to page 1
        click-events:
          previous-scene-click: {}
```

---

| ← Previous | Next → |
|:---|---:|
| [**Creating GUIs**](Creating-GUIs.md) | [**Click Events**](Click-Events.md) |
