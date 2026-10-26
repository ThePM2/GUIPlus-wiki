> ⚙ **GETTING STARTED**

# Getting Started

*Install GUIPlus, claim your first reward and create a menu.*

---

## 📥 Installation

1. Use a supported server: Paper or Spigot 1.21.11, or Paper 26.3.
2. Install the Java version required by the server. GUIPlus needs Java 21 or newer; Paper 26.x needs Java 25 or newer.
3. Place one `GUIPlus-4.0.0.jar` in `plugins/` and restart the server.
4. Check the console for startup errors before opening a menu.

The first start may download libraries, so the server needs access to their repositories. For an existing installation, follow [Updating & Backups](Updating-and-Backups.md) first.

> **Note:** Install the plugin with a full server restart. Use `/gui reload` later for menu changes, not the server's `/reload` command.

## 🎁 Try the Included Rewards

On a fresh installation, type:

```text
/rewards
```

Choose Daily Rewards, Playtime Milestones or Starter Choice. The first daily reward is ready immediately, and all three menus give real vanilla items without extra plugins.

For an existing server, an administrator can add the missing menus:

```text
/gui practical install
```

Existing menu files and saved progress are kept. If another plugin already uses `/rewards`, use `/guiplus-rewards` instead.

To change the actual reward items, use `/gui practical edit`. Editing a menu item's name or lore changes its appearance; it does not change the prize bundle. See [Ready-to-Use Rewards](Ready-to-Use-Rewards.md).

## 📂 Files and Folders

GUIPlus keeps its data in `plugins/GUIPlus/`:

| File or folder | Contents |
|----------------|----------|
| `config.yml`, `messages.yml` | Main settings and messages. |
| `CustomGuis/` | Your menu definitions. |
| `SystemMenus/` | Layouts for the in-game editors. |
| `playerData.yml`, `cooldownData.yml` | Saved player fields and cooldowns. |
| `playerStorage/` | Personal storage, one file per player UUID. |
| `PracticalRewards.yml` | Actual reward bundles and timing. |
| `SharedComponents.yml` | Shared item templates and previous revisions. |
| `WarpCatalogs.yml` | Warp catalog layouts and appearance. |
| `backups/` | Automatic GUIPlus backups. |

Some data files appear when the relevant feature is first used. Keep them with your menus when backing up the plugin.

## 🖼 Create Your First Menu

1. Run `/gui` as an administrator.
2. Choose **Create GUI** and give the menu a new ID.
3. Set its title, inventory type and rows.
4. Open the item editor to change names, lore, actions and conditions.
5. Save the menu with `/gui save`.

You can also start from an editable authoring example:

```text
/gui examples progression my_progress
/gui examples unlocks my_unlocks
/gui examples choices my_choices
```

These are demonstrations you can adapt. The progression example does not grant a real external rank or money. The working reward menus are opened separately with `/rewards`.

## 📝 Create a Menu with YAML

Create `plugins/GUIPlus/CustomGuis/welcome.yml`:

```yaml
id: welcome
type: chest
rows: 3
title: '&6Welcome'
commandAlias: welcome
scenes:
  '0':
    delay: 0
    items:
      '1':
        slot: 13
        item: DIAMOND
        amount: 1
        item-name: '&bHello!'
        item-lore:
          - '&7Click for a welcome message.'
        click-events:
          message:
            message: '&aWelcome to the server!'
```

Run `/gui reload`, then `/gui open welcome` or `/welcome`. The file name and menu ID should each be unique.

## 🚀 Next Steps

- [Creating GUIs](Creating-GUIs.md) covers item properties, colors and MiniMessage titles.
- [Click Events](Click-Events.md) and [Conditions](Conditions.md) add behavior.
- [Shared Components](Shared-Components.md) lets two menus use the same button.
- [Warp Catalogs](Warp-Catalogs.md) builds a menu from your EssentialsX destinations.

---

| ← Previous | Next → |
|:---|---:|
| [**Home**](Home.md) | [**Commands & Permissions**](Commands-and-Permissions.md) |
