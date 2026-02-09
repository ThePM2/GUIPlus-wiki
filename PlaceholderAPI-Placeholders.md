> 🔌 **INTEGRATIONS**

# PlaceholderAPI Placeholders

*Dynamic content and custom GUIPlus placeholders*

---

GUIPlus integrates with [PlaceholderAPI](https://www.spigotmc.org/resources/placeholderapi.6245/) to provide dynamic content in GUIs and registers its own custom placeholders.

## 📝 Using Placeholders in GUIs

PlaceholderAPI placeholders can be used in nearly every text field:

- Item display names
- Item lore lines
- Click event parameters (commands, messages, amounts)
- Condition values
- Skull textures
- Chat fetcher messages

### Example: Dynamic Lore

```yaml
item-name: §6%player_name%'s Profile
item-lore:
  - ''
  - §7Balance: §a$%vault_eco_balance_formatted%
  - §7Level: §e%player_level%
  - §7Health: §c%player_health%
  - ''
  - §7Bank: §b$%GUIPlus_player_info_deposited%
```

Lore lines containing placeholders are updated automatically while the GUI is open (every 20 ticks / 1 second).

## 🔗 GUIPlus Custom Placeholders

GUIPlus registers the following custom placeholders under the identifier `GUIPlus`:

| Placeholder | Description | Example |
|-------------|-------------|---------|
| `%GUIPlus_player_info_<field>%` | Returns a stored player data field value | `%GUIPlus_player_info_kills%` → `25` |
| `%GUIPlus_gui_exist_<id>%` | Returns `true` or `false` if a GUI with the given ID exists | `%GUIPlus_gui_exist_bank%` → `true` |
| `%GUIPlus_gui_title_<id>%` | Returns the title of the specified GUI | `%GUIPlus_gui_title_bank%` → `§ePersonal Bank` |
| `%GUIPlus_gui_rows_<id>%` | Returns the number of rows in the specified GUI | `%GUIPlus_gui_rows_bank%` → `3` |
| `%GUIPlus_gui_type_<id>%` | Returns the inventory type of the specified GUI | `%GUIPlus_gui_type_bank%` → `CHEST` |
| `%GUIPlus_gui_scenes_<id>%` | Returns the number of scenes in the specified GUI | `%GUIPlus_gui_scenes_bank%` → `1` |
| `%GUIPlus_self_base64%` | Returns the player's skin texture as a Base64 string (for skull items) | Used in `skullBase64` field |

## 💾 Player Info Placeholder

The `%GUIPlus_player_info_<field>%` placeholder is the most commonly used GUIPlus placeholder. It retrieves data saved via the [Save Player Info click event](Click-Events#save-player-info).

```yaml
# Saving a value
click-events:
  save-player-info:
    save-format: coins:100

# Displaying it in lore
item-lore:
  - §7Your coins: §e%GUIPlus_player_info_coins%
```

If the field has not been set for a player, the placeholder returns the string `null`.

See [Player Data](Player-Data) for full details on saving and retrieving data.

## ⭐ Special Placeholders

### `%input%`

Available inside [Chat Fetcher](Chat-Fetcher) nested click events and conditions. Contains the text the player typed in chat.

```yaml
click-events:
  chat-fetcher:
    message: §eType your message:
    click-events:
      message:
        message: §7You said: §f%input%
```

### `%player%`

The name of the clicking player. Available in all click events.

### `%executor%`

The name of the player who executed the action. Useful in player picker click events where `%player%` refers to the selected player.

### `%self_base64%`

Used in skull textures to dynamically render the viewing player's skin. See [Custom Heads](Custom-Heads-and-Skulls).

## 🧩 Combining with Other PAPI Plugins

Since GUIPlus supports any PlaceholderAPI placeholder, you can use placeholders from any installed PAPI expansion:

```yaml
item-lore:
  - §7Play time: §e%statistic_time_played%
  - §7Rank: §a%luckperms_primary_group%
  - §7Kills: §c%statistic_player_kills%
  - §7Deaths: §4%statistic_deaths%
  - §7K/D: §6%math_{statistic_player_kills}/{statistic_deaths}%
```

---

| ← Previous | Next → |
|:---|---:|
| [**Custom Heads & Skulls**](Custom-Heads-and-Skulls) | [**BungeeCord Support**](BungeeCord-Support) |
