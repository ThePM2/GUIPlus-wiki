> 🎨 **CREATING GUIS**

# Animations

*Use timed scenes and changing lore to add movement to a menu.*

---

## 🎬 Timed Scenes

Create several scenes in the editor, change the visible items between them and give each scene a delay. Leave **Disable scene animation** off to play the sequence when the GUI opens.

This example changes a gray item into a green one after one second:

```yaml
id: welcome_animation
type: chest
rows: 3
title: '&8Welcome'
disable-scene-animation: false
scenes:
  '0':
    delay: 20
    items:
      '1':
        slot: 13
        item: GRAY_DYE
        item-name: '&7Welcome...'
  '1':
    delay: 0
    items:
      '1':
        slot: 13
        item: LIME_DYE
        item-name: '&aEnjoy your stay!'
```

The first scene opens immediately. Its `delay: 20` schedules the next scene about one second later. Each later transition adds the previous scene's delay. The sequence ends on its final scene; it does not loop automatically.

Use `disable-scene-animation: true` for ordinary pages that should wait for a Next button. See [Scenes](Scenes.md).

## 📝 Animated Lore

Open an item's lore editor and add more pages. Each page has its own delay. At runtime, GUIPlus changes the lore while the same menu stays open.

For YAML, put `item-lore-data` inside the item. Page IDs start at `'1'`:

```yaml
item-lore-data:
  '1':
    delay: 20
    lore:
      - '&eClick to read the rules.'
  '2':
    delay: 40
    lore:
      - '&7Be kind and keep chat friendly.'
```

The first page lasts about one second, the second about two seconds, then the lore cycles back. `item-lore` supplies the ordinary first-page lore when you are not using a separate page definition.

## ⏱ Timing

| Setting | Unit |
|---------|------|
| Scene `delay` | Ticks before the next automatic scene transition. |
| Lore-page `delay` | Ticks that page stays visible. |
| Action `executionDelay` | Ticks before that action runs. |
| `gui-settings.update-delay` | Ticks between dynamic text refreshes. |
| Cooldown condition `cooldown` | Milliseconds. |

There are normally 20 server ticks per second. Under server lag, tick-based animations take longer in real time. Dynamic placeholder refresh and lore-page animation use separate settings.

## ✨ Practical Tips

- Build and test the static menu first, then add motion.
- Use the same size and inventory type for scenes that should switch seamlessly.
- Keep navigation and important text readable long enough to use.
- A scene's admission conditions are checked when it is actually opened. A scheduled transition cannot bypass its permission requirement.

For an attention effect on a particular slot, see [Glow At Item](Click-Events.md#glow-at-item).
