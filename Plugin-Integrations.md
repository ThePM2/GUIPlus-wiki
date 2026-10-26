> 🔌 **INTEGRATIONS**

# Plugin Integrations

*Connect menus to economy, permissions, custom items and other server plugins*

---

GUIPlus connects to other plugins in several ways. A direct integration uses a provider API; a command or placeholder example uses the commands or PlaceholderAPI data supplied by another plugin. Installing GUIPlus does not install those providers or grant their permissions.

| Connection | What GUIPlus provides |
|---|---|
| Vault | Economy actions, balance conditions and bank workflows through an active economy provider |
| PlayerPoints | A points requirement and a points-removal action when PlayerPoints is available |
| PlaceholderAPI | External placeholders in supported menu text and conditions |
| HeadDatabase | Head IDs in `skullBase64` and the item editor |
| EssentialsX warp catalog | An automatic warp browser with EssentialsX teleport rules |
| Proxy plugin messaging | The `server-click-event` action through the `BungeeCord` channel |
| Other plugin commands | Player or console command dispatch using configured command text |

## 💰 Vault & Economy Plugins

**Required for:** `money-give`, `money-remove`, `money-set`, `has-money` and Vault-backed bank transfers.

Install [Vault](https://www.spigotmc.org/resources/vault.34315/) and an economy plugin that registers a Vault economy provider, such as EssentialsX Economy. Vault by itself does not provide balances. GUIPlus detects the active provider automatically.

### Example: Fixed-Price Native Item

This item fragment charges $500 before delivering an ordinary sword through GUIPlus. The displayed icon can be configured separately.

```yaml
conditions:
  has-money:
    required-balance: 500
  has-empty-slots:
    free-slots: 1
conditionFailMessage: '&cYou need $500 and one empty inventory slot.'
click-events:
  money-remove:
    amount: 500
  custom-item-give:
    item:
      type: DIAMOND_SWORD
      amount: 1
  message:
    message: '&aPurchased a Diamond Sword for $500!'
```

Use [Player Data](Player-Data.md) for GUIPlus deposits and withdrawals. For shop commands supplied by another plugin, prefer that plugin's own purchase command when it handles charging and delivery together. A GUIPlus action list cannot make an arbitrary external command and economy operation share one transaction.

---

## 💎 PlayerPoints

PlayerPoints is a separate points integration. It is not a Vault economy provider merely because it is installed.

```yaml
conditions:
  player-points:
    amount: 100
conditionFailMessage: '&cYou need 100 points.'
click-events:
  player-points-remove:
    amount: 100
  custom-item-give:
    item:
      type: EMERALD
      amount: 1
```

Install and enable PlayerPoints before loading a GUI that uses these types. They are registered only when that integration is available. `%GUIPlus_player_info_points%` reads a GUIPlus custom field; it is not the PlayerPoints balance.

---

## 🔑 LuckPerms

[LuckPerms](https://luckperms.net/) supplies permissions through Bukkit, which GUIPlus's `has-permission` condition checks. No special GUIPlus LuckPerms hook is required.

### Permission-Based Items

Grant an explicit node such as `server.vip` to the intended group. `showConditions` hides the button; `conditions` guards the click:

```yaml
showConditions:
  has-permission:
    permission: server.vip
conditions:
  has-permission:
    permission: server.vip
conditionFailMessage: '&cVIP only!'
```

For a visible locked appearance, see [Button States](Button-States.md).

### Rank Display

With PlaceholderAPI and the matching LuckPerms placeholder provider enabled:

```yaml
item-lore:
  - '&7Rank: &f%luckperms_primary_group%'
  - '&7Prefix: %luckperms_prefix%'
```

Confirm that each placeholder resolves on your server before placing it in a requirement.

---

## 🧰 EssentialsX

[EssentialsX](https://essentialsx.net/) commands can be used for homes, warps and kits. These examples run commands as the clicking player, so EssentialsX controls permission, cooldown, cost and destination handling.

```yaml
click-events:
  command:
    commands:
      - 'essentials:warp mines'
  close-inventory: {}
```

Replace the command with `'essentials:home'` or `'essentials:kit starter'` for the other use cases. A command being dispatched does not prove that EssentialsX accepted it, so avoid adding unconditional messages such as “Kit received.”

For a menu that follows the actual warp list, open `/gui warps` and use [Warp Catalogs](Warp-Catalogs.md). That feature reads EssentialsX warps and delegates teleporting to EssentialsX rather than copying coordinates into every button.

---

## 🎨 ItemsAdder

[ItemsAdder](https://www.spigotmc.org/resources/itemsadder.73355/) supplies its own custom items and resource pack. GUIPlus can display a copied custom item configured through the item editor, use placeholder text supplied by the plugin, or dispatch a configured command.

An example command action is:

```yaml
click-events:
  console_command:
    commands:
      - 'iagive %player% custom_namespace:custom_item 1'
```

Replace the item ID and confirm the command syntax against your installed ItemsAdder version. This is command dispatch, not an ItemsAdder purchase API in GUIPlus.

For a resource-pack model defined on your server:

```yaml
item: DIAMOND
item-custom-model-data: 10001
```

The item-model field is also available on supported Minecraft versions:

```yaml
item: DIAMOND
item-model: 'mynamespace:custom_diamond'
```

A matching client resource pack must define that model. On versions that support custom-model-data components, use the item editor to capture the complete model data.

---

## 🎭 Oraxen

[Oraxen](https://www.spigotmc.org/resources/oraxen.72448/) content can be used through copied items, configured commands and placeholders supplied by its integration with PlaceholderAPI.

```yaml
click-events:
  console_command:
    commands:
      - 'oraxen give %player% custom_sword 1'
```

Replace the sample item ID and confirm the command for your installed version. For glyphs, use a placeholder provided by your Oraxen setup in a quoted `item-name` or lore line. GUIPlus does not install the glyphs or resource pack.

---

## 🛡 WorldGuard

GUIPlus has no direct WorldGuard condition. If your PlaceholderAPI setup provides a region placeholder, use it in `conditional-placeholder`. For example, with a provider that resolves `%worldguard_region_name%`:

```yaml
showConditions:
  conditional-placeholder:
    conditional_condition: '%worldguard_region_name%(=)spawn'
```

Check the provider's handling of overlapping regions and missing regions before using its result for access control. To guard an action as well, repeat the requirement under `conditions`.

---

## 🧑 Citizens / NPCs

[Citizens](https://www.spigotmc.org/resources/citizens.13811/) and other NPC plugins can open a GUI by running a command when a player interacts. Configure a **player-executed** command using the menu's alias, or:

```
gui open shop
```

For that command, the player needs `guiplus.gui.open` plus the menu's own permission. If your NPC plugin runs a command as console instead, pass the interacting player's name using the NPC plugin's player token:

```
gui open shop <interacting-player>
```

`<interacting-player>` above is a configuration placeholder to replace with your NPC plugin's token. NPC command setup and token syntax belong to the NPC plugin.

---

## 💬 DiscordSRV

[DiscordSRV](https://www.spigotmc.org/resources/discordsrv.18494/) information can appear in names and lore when an installed PlaceholderAPI provider exposes it. For example, if your provider resolves this token:

```yaml
item-lore:
  - '&7Discord: &9%discordsrv_user_tag%'
```

GUIPlus does not link Discord accounts or send Discord messages through this example.

---

## ⚔ Towny / Factions

Use the placeholder provider that matches your town or faction plugin. Expansion names and tokens differ between implementations. Example lore for a provider exposing these tokens:

```yaml
item-lore:
  - '&7Town: &a%townyadvanced_town%'
  - '&7Nation: &b%townyadvanced_nation%'
```

```yaml
item-lore:
  - '&7Faction: &a%factionsuuid_faction_name%'
  - '&7Power: &c%factionsuuid_faction_power%'
```

Check each token with `/papi parse me <placeholder>`. An unresolved token is text, not usable numeric data for a requirement.

---

## 💀 HeadDatabase

[HeadDatabase](https://www.spigotmc.org/resources/head-database.14280/) is a direct optional integration. Use an available database head ID in `skullBase64`:

```yaml
item: PLAYER_HEAD
skullBase64: 'hdb-123'
```

Replace `123` with an ID available in your database. You can also paste a Base64 texture without HeadDatabase. See [Custom Heads & Skulls](Custom-Heads-and-Skulls.md) for complete examples.

---

## 🌍 Multiverse-Core

[Multiverse-Core](https://www.spigotmc.org/resources/multiverse-core.390/) can load and manage worlds used by GUIPlus. Its soft-dependency entry helps load ordering; the native `teleport` action uses Bukkit coordinates rather than a Multiverse teleport command.

```yaml
click-events:
  teleport:
    location: 'World: world_nether, X: 0.5, Y: 65.0, Z: 0.5, Yaw: 0.0, Pitch: 0.0'
```

Choose a safe destination in a loaded world. A bare world name is not the saved YAML format. If you want Multiverse's own teleport behavior, dispatch its documented command instead.

For a world-dependent display, use an installed world placeholder provider. For example, with the Player expansion:

```yaml
showConditions:
  conditional-placeholder:
    conditional_condition: '%player_world%(=)world'
```

---

## 🌐 PAPIProxyBridge

[PAPIProxyBridge](https://www.spigotmc.org/resources/papiproxybridge.108415/) can supply cross-server placeholders to your PlaceholderAPI setup. Use the tokens documented for your installed bridge version in menu text, then combine them with a native server action:

```yaml
item-name: '&aSurvival'
item-lore:
  - '&7Click to join the Survival server.'
click-events:
  server-click-event:
    server: survival
```

The bridge supplies display data; it does not make `server-click-event` work without a correctly configured proxy. See [BungeeCord Support](BungeeCord-Support.md).

---

## 🔧 General Integration Pattern

1. Confirm the provider's command or placeholder works outside the GUI.
2. Use `command` for player authority or `console_command` for console authority. Write commands without the leading `/`.
3. Put display requirements under `showConditions`, and click requirements under `conditions`.
4. Use explicit `type` values when giving actions your own names.

```yaml
item-lore:
  - '&7Tokens: &f%otherplugin_tokens%'
conditions:
  conditional-placeholder:
    conditional_condition: '%otherplugin_tokens%>=10'
click-events:
  open-token-menu:
    type: command
    commands:
      - 'otherplugin tokens'
```

Replace the sample token and command with real values. Numeric comparisons reject unavailable or invalid numeric data. Unknown action or condition types also prevent the affected GUI from loading; restore the providing integration or correct the type before reloading.

---

| ← Previous | Next → |
|:---|---:|
| [**BungeeCord Support**](BungeeCord-Support.md) | [**DeluxeMenus Converter**](DeluxeMenus-Converter.md) |
