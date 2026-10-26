> 💬 **INTERACTIVE**

# Custom Heads & Skulls

*Use Base64 textures and dynamic player skins in your GUIs*

---

GUIPlus supports custom player head textures using Base64-encoded skin data. This allows you to use decorative heads, player avatars, and custom icons in your GUIs.

## 🔑 Using Base64 Skull Textures

Set the item material to `PLAYER_HEAD` and add the `skullBase64` property with a Base64-encoded texture string:

```yaml
'1':
  slot: 13
  item: PLAYER_HEAD
  amount: 1
  item-name: '§6Custom Head'
  skullBase64: 'eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvOWZkMTA4MzgzZGZhNWIwMmU4NjYzNTYwOTU0MTUyMGU0ZTE1ODk1MmQ2OGMxYzhmOGYyMDBlYzdlODg2NDJkIn19fQ=='
```

## 🔍 Finding Base64 Textures

You can find Base64 skull textures from these resources:

- [minecraft-heads.com](https://minecraft-heads.com/) — Browse thousands of custom heads and copy their Base64 value
- [Freshcoal Heads](https://freshcoal.com/maincollection) — Another collection of decorative heads

On minecraft-heads.com, look for the "Value" field in the head's detail page — that's the Base64 string you need.

## 👤 Dynamic Player Skin

With PlaceholderAPI enabled, use `%GUIPlus_self_base64%` to show the viewing player's own skin:

```yaml
'1':
  slot: 4
  item: PLAYER_HEAD
  amount: 1
  item-name: '§6%player_name%''s Profile'
  item-lore:
    - ''
    - '§7Balance: §a$%vault_eco_balance_formatted%'
    - '§7Level: §e%player_level%'
  skullBase64: '%GUIPlus_self_base64%'
```

This dynamically renders each player's own head, making it ideal for profile menus and player info displays.

Use the full `%GUIPlus_self_base64%` token. GUIPlus provides it through its PlaceholderAPI expansion. The `%player_name%`, Vault, LuckPerms and Statistic tokens shown on this page need their corresponding PlaceholderAPI providers.

## 🧩 HeadDatabase Integration

If you have [HeadDatabase](https://www.spigotmc.org/resources/head-database.14280/) installed, use `hdb-<id>` in `skullBase64`:

```yaml
'1':
  slot: 13
  item: PLAYER_HEAD
  amount: 1
  item-name: '&6Database Head'
  skullBase64: 'hdb-123'
```

Replace `123` with a valid ID from your installed database. GUIPlus resolves that head through HeadDatabase; a later save may store its resolved Base64 texture. You can also enter an HDB ID in the item editor. Raw Base64 textures do not require HeadDatabase.

## 👤 Example: Player Profile Menu

This menu uses PlaceholderAPI with the Player, LuckPerms, Vault and Statistic providers. Remove or replace any lore token your server does not provide.

```yaml
id: profile
rows: 3
type: chest
title: '§6Player Profile'
commandAlias: profile
scenes:
  '0':
    delay: 0
    items:
      '1':
        slot: 4
        item: PLAYER_HEAD
        amount: 1
        item-name: '§e§l%player_name%'
        item-lore:
          - ''
          - '§7Rank: §a%luckperms_primary_group%'
          - '§7Balance: §6$%vault_eco_balance_formatted%'
          - '§7Play Time: §b%statistic_time_played%'
          - ''
        skullBase64: '%GUIPlus_self_base64%'
      '2':
        slot: 10
        item: PLAYER_HEAD
        amount: 1
        item-name: '§aInformation'
        skullBase64: 'eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvOTZhM2UzYzNhMzlhNGNkYjk1MTk5OTY5OTM0OGEyNjY4YTZlZjBjMmY0YjNlMGRkMzI2OGI5MzVkNmEyIn19fQ=='
        item-lore:
          - ''
          - '§7Server: §eSurvival'
          - '§7Online: §a%server_online%§7/§a%server_max_players%'
          - ''
      '3':
        slot: 16
        item: PLAYER_HEAD
        amount: 1
        item-name: '§cSettings'
        skullBase64: 'eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvZTRkNDliYTc4ODY3YmE2M2JjMzhjZjk4NGM0NWQ0NTI4NjBjNzlmMGU3MjIxMjI1YzNhNWQ1N2M5MTQ2MSJ9fX0='
        item-lore:
          - ''
          - '§7Add your settings menu link here'
          - ''
```

## 🔍 Troubleshooting

- Use `PLAYER_HEAD`; other materials do not render a player skin.
- Paste the complete Base64 value, preserving its case. A texture URL or a `/give` command is not the Base64 field.
- Quote Base64 values and placeholders in YAML.
- For a dynamic skin, confirm `/papi parse me %GUIPlus_self_base64%` resolves and the player has a skin texture available.
- For `hdb-` entries, confirm the plugin is enabled and the ID exists. If HeadDatabase cannot load the texture, check the console and fix the integration before saving the affected GUI.

---

| ← Previous | Next → |
|:---|---:|
| [**Player Data**](Player-Data.md) | [**PlaceholderAPI Placeholders**](PlaceholderAPI-Placeholders.md) |
