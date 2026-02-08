# GUIPlus

Create fully customizable GUIs for your Minecraft server — no coding required.

GUIPlus is a powerful GUI utility plugin that allows server administrators to build complex, interactive inventory menus through simple YAML configuration. From basic server menus to advanced banking systems with conditions and multi-scene navigation, GUIPlus handles it all.

## Key Features

- **In-Game GUI Editor** — Create and edit GUIs directly in-game with an intuitive editor interface
- **25 Click Event Types** — Messages, commands, economy actions, teleportation, scene navigation, and more
- **11 Condition Types** — Control item visibility and click execution with permission checks, balance requirements, placeholders, cooldowns, and more
- **Multi-Scene Support** — Build multi-page GUIs with independent layouts and navigation
- **PlaceholderAPI Integration** — Dynamic content that updates in real-time using any PAPI placeholder
- **Chat Fetcher System** — Prompt players for input via chat, then use that input in click events
- **Player Data Storage** — Save and retrieve per-player data fields accessible via placeholders
- **Custom Skull Textures** — Use Base64 textures or dynamic player skins with `%self_base64%`
- **Economy Support** — Give, remove, or set player balances with Vault integration
- **BungeeCord Support** — Send players to other servers in your network
- **DeluxeMenus Converter** — Automatically convert existing DeluxeMenus configurations
- **4 Inventory Types** — Chest, Dispenser, Dropper, and Hopper

## Supported Versions

- **Minecraft:** 1.16+
- **Server Software:** Spigot, Paper
- **Java:** 17+

## Soft Dependencies

| Plugin | Purpose |
|--------|---------|
| [Vault](https://www.spigotmc.org/resources/vault.34315/) | Economy support (give/remove/check money) |
| [PlaceholderAPI](https://www.spigotmc.org/resources/placeholderapi.6245/) | Dynamic placeholder replacement |
| [PlayerPoints](https://www.spigotmc.org/resources/playerpoints.80745/) | PlayerPoints currency support |
| [HeadDatabase](https://www.spigotmc.org/resources/head-database.14280/) | Custom head retrieval |
| [Multiverse-Core](https://www.spigotmc.org/resources/multiverse-core.390/) | Multi-world support |

## Quick Links

**Getting Started**
- [Getting Started](Getting-Started) — Installation, default files, first GUI tutorial
- [Commands & Permissions](Commands-and-Permissions) — All commands and permission nodes
- [Configuration](Configuration) — Every config.yml option explained

**Features**
- [Creating GUIs](Creating-GUIs) — GUI structure, inventory types, item properties
- [Click Events](Click-Events) — All 25 click event types with examples
- [Conditions](Conditions) — All 11 condition types for visibility and validation
- [Scenes](Scenes) — Multi-page GUI navigation
- [Chat Fetcher](Chat-Fetcher) — Player input prompts and validation
- [Player Data](Player-Data) — Per-player data storage and math operations

**Integrations**
- [PlaceholderAPI Placeholders](PlaceholderAPI-Placeholders) — Custom placeholders and dynamic content
- [Custom Heads & Skulls](Custom-Heads-and-Skulls) — Base64 textures and player skins
- [BungeeCord Support](BungeeCord-Support) — Cross-server navigation
- [DeluxeMenus Converter](DeluxeMenus-Converter) — Migration from DeluxeMenus

**Help & Development**
- [FAQ & Troubleshooting](FAQ-and-Troubleshooting) — Common questions and solutions
- [Developer API](Developer-API) — Java API for plugin developers
