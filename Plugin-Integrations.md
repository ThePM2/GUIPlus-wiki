> 🔌 **INTEGRATIONS**

# Plugin Integrations

*How GUIPlus works with popular Minecraft plugins*

---

GUIPlus integrates with a wide range of plugins through PlaceholderAPI placeholders, Vault economy, and command execution. This page documents how to set up and use GUIPlus with popular plugins.

## 💰 Vault & Economy Plugins

**Required for:** `money-give`, `money-remove`, `money-set`, `has-money` condition

GUIPlus uses [Vault](https://www.spigotmc.org/resources/vault.34315/) as a bridge to any economy plugin. Install Vault plus one of these:

| Economy Plugin | Notes |
|---------------|-------|
| EssentialsX | Most common, works out of the box |
| CMI Economy | Included with CMI |
| PlayerPoints | Supported natively with `player-points-remove` click event |

### Setup

1. Install Vault and your economy plugin
2. No GUIPlus configuration needed — Vault is detected automatically

### Example: Shop Item

```yaml
conditions:
  has-money:
    required-balance: 500
conditionFailMessage: §cYou need $500!
click-events:
  money-remove:
    amount: 500
  console_command:
    commands:
      - give %player% diamond_sword 1
  message:
    message: §aPurchased for $500!
```

---

## 🔑 LuckPerms

**Used for:** Permission-based conditions, rank display in lore

[LuckPerms](https://luckperms.net/) is the most popular permissions plugin. GUIPlus works with it through permission conditions and PlaceholderAPI.

### Display Rank in GUI

Install the LuckPerms PAPI expansion: `/papi ecloud download LuckPerms`

```yaml
item-lore:
  - §7Rank: §f%luckperms_primary_group%
  - §7Prefix: %luckperms_prefix%
```

### Permission-Based Items

Show items only to specific ranks:

```yaml
conditions:
  has-permission:
    permission: group.vip
conditionFailMessage: §cVIP only!
```

### Rank-Specific Menus

Create different menu experiences per rank by using conditions on items. VIP players see upgraded items while regular players see defaults. Use sorting groups (via the in-game editor) to set fallback items on the same slot.

---

## 🧰 EssentialsX

**Used for:** Warps, homes, teleportation, kits

[EssentialsX](https://essentialsx.net/) provides many commands that work with GUIPlus click events.

### Warp Menu

```yaml
click-events:
  command:
    commands:
      - warp mines
  close-inventory: {}
```

### Kit Selection

```yaml
click-events:
  console_command:
    commands:
      - essentials:kit starter %player%
  message:
    message: §aKit applied!
```

### Home Teleport

```yaml
click-events:
  command:
    commands:
      - home
  close-inventory: {}
```

### Display Player Info

Install the Essentials PAPI expansion: `/papi ecloud download Essentials`

```yaml
item-lore:
  - §7Nickname: %essentials_nickname%
  - §7AFK: %essentials_afk%
  - §7God Mode: %essentials_godmode%
```

---

## 🎨 ItemsAdder

**Used for:** Custom items, custom textures, custom blocks in GUIs

[ItemsAdder](https://www.spigotmc.org/resources/itemsadder.73355/) lets you use custom items and font images in your GUIs.

### Custom Items via Console Command

Since GUIPlus supports any console command, you can give ItemsAdder items:

```yaml
click-events:
  console_command:
    commands:
      - iagive %player% custom_namespace:custom_item 1
```

### Custom Font Images in Lore

If you have the ItemsAdder PAPI expansion, you can use font images:

```yaml
item-lore:
  - '%img_coin% §7Price: §a500 coins'
  - '%img_star% §7Rarity: §6Legendary'
```

### Custom Model Data

For resource pack items, use the `item-custom-model-data` property:

```yaml
item: DIAMOND
item-custom-model-data: 10001
```

On Minecraft 1.21.2+, you can also use the `item-model` property:

```yaml
item: DIAMOND
item-model: 'mynamespace:custom_diamond'
```

---

## 🎭 Oraxen

**Used for:** Custom items, custom textures, glyphs

[Oraxen](https://www.spigotmc.org/resources/oraxen.72448/) is similar to ItemsAdder for custom content.

### Custom Items

```yaml
click-events:
  console_command:
    commands:
      - oraxen give %player% custom_sword 1
```

### Oraxen Glyphs in Text

With the Oraxen PAPI expansion, use glyph placeholders in item names and lore:

```yaml
item-name: '%oraxen_coin% §6Premium Shop'
```

---

## 🛡 WorldGuard

**Used for:** Region-based conditions, protected area menus

While GUIPlus doesn't have built-in WorldGuard support, you can use WorldGuard's PlaceholderAPI expansion for region-aware GUIs.

### Install

`/papi ecloud download WorldGuard`

### Region-Based Items

Show items only when a player is in a specific region:

```yaml
conditions:
  conditional-placeholder:
    conditional_condition: '%worldguard_region_name%(=)spawn'
```

---

## 🧑 Citizens / NPCs

**Used for:** Opening GUIs from NPC interactions

[Citizens](https://www.spigotmc.org/resources/citizens.13811/) NPCs can open GUIPlus menus when players interact with them.

### Using Citizens Commands

Add a command to the NPC that runs when right-clicked:

```
/npc select
/npc command add -p gui open shop
```

The `-p` flag makes the player run the command. The NPC will open the `shop` GUI when clicked.

### Using Other NPC Plugins

Any NPC plugin that supports running commands on interaction works with GUIPlus. Configure the NPC to run `/gui open <name>` on click.

---

## 💬 DiscordSRV

**Used for:** Displaying Discord-linked info in GUIs

With [DiscordSRV](https://www.spigotmc.org/resources/discordsrv.18494/) and its PAPI expansion, you can show Discord information:

```yaml
item-lore:
  - §7Discord: §9%discordsrv_user_tag%
```

---

## ⚔ Towny / Factions

**Used for:** Displaying faction/town info in GUIs

### Towny

Install the PAPI expansion: `/papi ecloud download TownyAdvanced`

```yaml
item-lore:
  - §7Town: §a%townyadvanced_town%
  - §7Nation: §b%townyadvanced_nation%
  - §7Residents: §f%townyadvanced_town_residents%
```

### Factions

Install the PAPI expansion: `/papi ecloud download Factions`

```yaml
item-lore:
  - §7Faction: §a%factionsuuid_faction_name%
  - §7Power: §c%factionsuuid_faction_power%
  - §7Members: §f%factionsuuid_faction_members%
```

---

## 💀 HeadDatabase

**Used for:** Accessing custom heads from the HeadDatabase library

[HeadDatabase](https://www.spigotmc.org/resources/head-database.14280/) is a soft dependency of GUIPlus. When installed, it provides access to thousands of decorative heads.

GUIPlus automatically integrates with HeadDatabase when it's present. You can also use Base64 textures directly — see [Custom Heads & Skulls](Custom-Heads-and-Skulls.md).

---

## 🌍 Multiverse-Core

**Used for:** Multi-world teleportation, world-specific GUIs

[Multiverse-Core](https://www.spigotmc.org/resources/multiverse-core.390/) is a soft dependency of GUIPlus.

### World Teleportation

Use the teleport click event with a world name:

```yaml
click-events:
  teleport:
    location: world_nether
```

### World-Specific Conditions

With the Multiverse PAPI expansion, show items based on the player's current world:

```yaml
conditions:
  conditional-placeholder:
    conditional_condition: '%multiverse_world_name%=world'
```

---

## 🌐 PAPIProxyBridge

**Used for:** Displaying cross-server data in BungeeCord/Velocity networks

[PAPIProxyBridge](https://www.spigotmc.org/resources/papiproxybridge.108415/) lets you use PlaceholderAPI across your network. Combine with [BungeeCord Support](BungeeCord-Support.md) for server selectors that show player counts.

```yaml
item-name: §aSurvival
item-lore:
  - ''
  - §7Players: §a%papiproxybridge_server_online_survival%
  - ''
  - §eClick to join!
click-events:
  server-click-event:
    server: survival
```

---

## 🔧 General Integration Pattern

Any plugin that provides either **PlaceholderAPI placeholders** or **commands** can be used with GUIPlus:

1. **Display data** — Use the plugin's PAPI placeholders in item names, lore, or conditions
2. **Execute actions** — Use `command` or `console_command` click events to run the plugin's commands
3. **Check requirements** — Use `conditional-placeholder` conditions with the plugin's PAPI placeholders

```yaml
# Generic pattern for any plugin integration
item-lore:
  - §7Some Value: §f%otherplugin_placeholder%
conditions:
  conditional-placeholder:
    conditional_condition: '%otherplugin_placeholder%>=10'
click-events:
  console_command:
    commands:
      - otherplugin command %player%
```

---

| ← Previous | Next → |
|:---|---:|
| [**BungeeCord Support**](BungeeCord-Support.md) | [**DeluxeMenus Converter**](DeluxeMenus-Converter.md) |
