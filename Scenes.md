> 🎨 **CREATING GUIS**

# Scenes

*Build manual pages or timed menu sequences.*

---

A scene is one layout inside a GUI. Use scenes for pages with navigation buttons, or let GUIPlus play them in sequence as an animation.

## 📐 Scene Structure

Scenes are stored under `scenes`, normally with consecutive IDs starting at `'0'`. Each scene has its own items and actions.

| Property | Meaning |
|----------|---------|
| `items` | Items displayed in this scene. |
| `delay` | Ticks this scene contributes before the next automatic transition. |
| `open-conditions` | Conditions the viewer must meet to enter the scene. |
| `open-events` | Actions run when the scene opens. |

> **Note:** `open-conditions` gates the whole scene. If it fails, the scene does not open; it does not merely suppress the opening actions.

## 🧭 Manual Pages

Set `disable-scene-animation: true` at the menu root so pages stay open until a player navigates. This complete example creates two pages:

```yaml
id: information
type: chest
rows: 3
title: '&8Server Information'
disable-scene-animation: true
scenes:
  '0':
    delay: 0
    items:
      '1':
        slot: 13
        item: BOOK
        item-name: '&eRules'
        item-lore:
          - '&7Be kind to other players.'
      '2':
        slot: 26
        item: ARROW
        item-name: '&aNext page'
        click-events:
          next-scene-click: {}
  '1':
    delay: 0
    items:
      '1':
        slot: 13
        item: COMPASS
        item-name: '&bTravel'
        item-lore:
          - '&7Use /guiplus-warps to browse destinations.'
      '2':
        slot: 18
        item: ARROW
        item-name: '&ePrevious page'
        click-events:
          previous-scene-click: {}
```

`next-scene-click` and `previous-scene-click` try the adjacent scene. They still check that scene's entry conditions; they do not automatically search past an inaccessible page.

## 📂 Opening Actions and Entry Conditions

Add this scene fragment for a staff page:

```yaml
open-conditions:
  has-permission:
    permission: myserver.staff
open-events:
  welcome-sound:
    type: sound-click-event
    sound: UI_BUTTON_CLICK
  welcome-message:
    type: message
    message: '&7Staff tools are ready.'
```

Custom action labels such as `welcome-sound` need a `type`. A registered ID can be used directly, for example `message:`. The same rule applies to condition labels with an explicit `condition`.

Use the scene's `open-events` for ordinary opening actions. **Glow At Item** runs on opening automatically; it is a separate item effect, not a general `openEvent` switch in the action editor.

## ⏱ Timed Scenes

Set `disable-scene-animation: false` to play eligible scenes automatically. The first accessible scene opens immediately. Its `delay` is added before the next transition; subsequent delays are cumulative. `20` ticks is about one second.

For example, scene `'0'` with `delay: 20` is shown for roughly one second before scene `'1'`. A delay on the final scene does not restart the sequence. See [Animations](Animations.md) for a complete example and animated lore.

## 🖼 Switching Between Pages

GUIPlus can reuse an open window when the next page has the same inventory type and size. The `gui-settings.seamless-switching` setting controls this. On Paper, `seamless-title-updates` also supports title changes in place; other cases reopen the window.

---

| ← Previous | Next → |
|:---|---:|
| [**Creating GUIs**](Creating-GUIs.md) | [**Click Events**](Click-Events.md) |
