# Getting Started

## Installation

1. Download the latest `GUIPlus.jar` file
2. Place it in your server's `plugins/` folder
3. Restart your server

> **Warning:** Always use a full server restart when installing for the first time. Do not use `/reload` as it can cause issues with plugin initialization.

4. The plugin will generate its default configuration files

## Default Files

After first startup, GUIPlus creates the following files and folders:

```
plugins/GUIPlus/
├── config.yml           # Main plugin configuration
├── messages.yml         # Customizable messages
├── playerData.yml       # Persistent player data storage
├── cooldownData.yml     # Cooldown tracking
└── CustomGuis/          # Your GUI configuration files
    ├── adminpanel.yml   # Example admin panel GUI
    └── bank.yml         # Example banking GUI
```

## Your First GUI

GUIPlus comes with two example GUIs to get you started:

### Opening the Examples

```
/gui open adminpanel
/gui open bank
```

Or use the built-in management menu:

```
/gui
```

### Using the In-Game Editor

The fastest way to create a GUI is through the in-game editor:

1. Run `/gui` to open the GUI management menu
2. Click to create a new GUI
3. Set the title, type, and number of rows
4. Place items and configure click events visually
5. Save your GUI with `/gui save`

### Creating a GUI via YAML

You can also create GUIs by writing YAML files directly. Create a new `.yml` file inside the `plugins/GUIPlus/CustomGuis/` folder:

```yaml
id: mygui
rows: 3
type: chest
title: §6My First GUI
commandAlias: mymenu
scenes:
  '0':
    delay: 0
    items:
      '1':
        slot: 13
        item: DIAMOND
        amount: 1
        item-name: §bHello World!
        item-lore:
          - ''
          - §7This is my first GUIPlus menu!
          - ''
          - §eClick me!
        item-flags: []
        unbreakable: false
        click-events:
          message:
            message: §aYou clicked the diamond!
```

Then reload with `/gui reload` and open it with `/gui open mygui` or `/mymenu`.

## Next Steps

- Learn about the full [GUI structure](Creating-GUIs)
- Explore all [click events](Click-Events)
- Add logic with [conditions](Conditions)
- Use dynamic content with [PlaceholderAPI](PlaceholderAPI-Placeholders)

---

[Previous: Home](Home) | [Next: Commands & Permissions](Commands-and-Permissions)
