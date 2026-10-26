# GUIPlus

*Create and edit inventory menus directly in Minecraft.*

---

GUIPlus lets you build server menus with an in-game editor or YAML. Add commands, conditions, animated scenes and player input, then reuse shared buttons across your menus.

These pages describe **GUIPlus 4.0.0**. If you have just installed it, try `/rewards` for working Daily Rewards, Playtime Milestones and a Starter Choice. You can change the actual prizes with `/gui practical edit`.

## ✨ Key Features

| Feature | What you can do |
|---------|-----------------|
| [In-game editor](Creating-GUIs.md) | Create menus, copy items, change appearance and configure actions without writing YAML. |
| [Scenes and animations](Scenes.md) | Build pages, timed sequences and animated lore. |
| [Click events](Click-Events.md) | Run commands, give items, use an economy, open another menu or ask for player input. |
| [Conditions](Conditions.md) | Check permissions, balances and saved values; choose which items appear and which actions can run. |
| [Button states](Button-States.md) | Give one button Locked, Ready and Owned states with shared text values. |
| [Interaction flows](Interaction-Flows.md) | Collect several answers, offer choices and let the player review before confirming. |
| [Input rules](Input-Rules.md) | Accept bounded numbers, player names, enum choices and validated text. |
| [Ready-to-use rewards](Ready-to-Use-Rewards.md) | Start with real vanilla rewards, then edit bundles and timing in game. |
| [Shared components](Shared-Components.md) | Link buttons to one template while keeping local names and lore. |
| [Warp catalogs](Warp-Catalogs.md) | Generate a searchable, paged menu from your EssentialsX warps. |
| [Personal storage](Personal-Storage.md) | Give players saved item storage with permission-based sizes. |
| [Player data](Player-Data.md) | Save per-player values for your own menus and progression. |
| [Custom items and heads](Custom-Heads-and-Skulls.md) | Use held-item metadata, custom textures and player heads. |
| [Integrations](Plugin-Integrations.md) | Connect menu actions to Vault, PlaceholderAPI and your other server plugins. |

## 📋 Requirements

| | GUIPlus 4.0.0 |
|---|---|
| **Server profiles** | Paper and Spigot 1.21.11; Paper 26.3 |
| **Java** | Java 21 or newer, and the Java version required by your server. Paper 26.x requires Java 25 or newer. |
| **Folia** | Not supported. |
| **Default rewards** | No economy plugin, PlaceholderAPI or resource pack required. |

Older GUIPlus downloads have their own requirements. Do not use an old release's Minecraft version list for the V4 JAR.

## 🧩 Optional Plugins

| Plugin | Used for |
|--------|----------|
| Vault and an economy provider | Balance conditions, money actions and banking. |
| PlaceholderAPI and the relevant expansions | Other plugins' placeholders and ordinary GUIPlus player-data display. |
| EssentialsX | The automatic warp catalog; other Essentials commands remain Essentials-owned. |
| PlayerPoints | Points conditions and removal actions. |
| HeadDatabase | Selecting heads from its database. |
| Multiverse-Core | Menus that use its worlds or commands. |

Only install an integration if your menu uses it. See [Plugin Integrations](Plugin-Integrations.md) for examples.

### ⚙ Getting Started

| | |
|---|---|
| [🚀 Getting Started](Getting-Started.md) | Installation, first reward and first custom menu. |
| [📋 Commands & Permissions](Commands-and-Permissions.md) | Player commands, authoring commands and access. |
| [⚙ Configuration](Configuration.md) | Main settings, backups and refresh intervals. |
| [🔄 Updating & Backups](Updating-and-Backups.md) | Update an existing server and keep a matching rollback copy. |

### 🎨 Creating GUIs

Start with [Creating GUIs](Creating-GUIs.md), then add [scenes](Scenes.md), [animations](Animations.md), [click events](Click-Events.md) and [conditions](Conditions.md). For reusable menus, see [Button States](Button-States.md) and [Shared Components](Shared-Components.md).

### 💬 Interactive Menus

[Chat Fetcher](Chat-Fetcher.md) handles a single prompt. [Interaction Flows](Interaction-Flows.md) collect several answers with a review screen. Both use [Input Rules](Input-Rules.md). [Player Data](Player-Data.md) stores values you want to keep after the menu closes.

### 📖 Practical Examples

Try [Ready-to-Use Rewards](Ready-to-Use-Rewards.md), an [EssentialsX Warp Catalog](Warp-Catalogs.md) or [Personal Storage](Personal-Storage.md). The [Tutorials](Tutorials.md) and [Premade Configurations](Premade-Configurations.md) cover custom menus. `/gui examples` installs editable progression, unlock and choice demonstrations.

### ❓ Help & Development

[FAQ & Troubleshooting](FAQ-and-Troubleshooting.md) · [Tips & Best Practices](Tips-and-Best-Practices.md) · [Developer API](Developer-API.md)

## 💬 Support

For help, use the plugin's Discord or your purchase platform. Include your GUIPlus version, server version and the relevant console error. For menu problems, include the smallest configuration that reproduces them.
