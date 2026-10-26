> 🔌 **INTEGRATIONS**

# PlaceholderAPI Placeholders

*Display live information and use values in the right context*

---

GUIPlus supports [PlaceholderAPI](https://www.spigotmc.org/resources/placeholderapi.6245/) in item names, lore, supported condition values, chat prompt text and dynamic skull textures. Install the expansion that provides a foreign placeholder, then use its documented token.

GUIPlus also has local input, state, reward and catalog tokens. These have their own scopes; they do not need a PlaceholderAPI expansion.

## 📝 Dynamic Names and Lore

```yaml
item-name: '&6Profile'
item-lore:
  - '&7Balance: &a%vault_eco_balance_formatted%'
  - '&7Level: &e%player_level%'
  - '&7Points: &b%GUIPlus_player_info_points%'
```

The relevant PlaceholderAPI expansions must be installed. Dynamic item text refreshes using `gui-settings.update-delay` in `config.yml`: default `20` ticks, allowed range `1`–`1200`. Static text and unchanged rendered items are skipped.

A field accepting text does not automatically support every placeholder system. For example, a command's native player aliases and a flow's answers are distinct from PlaceholderAPI expansion tokens.

## 🔗 GUIPlus PlaceholderAPI Expansion

GUIPlus registers its expansion when PlaceholderAPI is available. No separate GUIPlus eCloud download is needed.

| Placeholder | Result |
|-------------|--------|
| `%GUIPlus_player_info_<field>%` | A stored field for the player being evaluated; `null` when unset |
| `%GUIPlus_gui_exist_<id>%` | `true` or `false` |
| `%GUIPlus_gui_title_<id>%` | The GUI's configured title |
| `%GUIPlus_gui_rows_<id>%` | Its configured row count |
| `%GUIPlus_gui_type_<id>%` | `CHEST`, `DROPPER`, `DISPENSER` or `HOPPER` |
| `%GUIPlus_gui_scenes_<id>%` | Its number of scenes |
| `%GUIPlus_self_base64%` | The online player's skin texture, for a skull field |

Use the GUI **ID**, not its title, in GUI tokens. For example, `%GUIPlus_gui_exist_bank%` checks the GUI with `id: bank`. Player-data tokens use a **field name**, such as `%GUIPlus_player_info_points%`; they do not contain a GUI ID.

```yaml
skullBase64: '%GUIPlus_self_base64%'
```

See [Custom Heads & Skulls](Custom-Heads-and-Skulls.md) for texture setup.

## 💾 Player Data Tokens

Save a value using an action, then display it through PlaceholderAPI:

```yaml
click-events:
  save-player-info:
    save-format: 'points:number:100'
```

```yaml
item-lore:
  - '&7Your points: &e%GUIPlus_player_info_points%'
```

GUIPlus resolves its own player-info tokens **without PlaceholderAPI** in `conditional-placeholder` conditions and [Button States](Button-States.md) names/lore. Ordinary legacy item names/lore and existing-field calculations in `save-player-info` use PlaceholderAPI. See [Player Data](Player-Data.md).

Conditions normally evaluate the viewer, while a saved-data action writes to its actual target. A player-picker name substitution does not change this context.

## ⭐ Input and Player Aliases

| Token | Where it is available | Meaning |
|-------|-----------------------|---------|
| `%input%` | A Chat Fetcher's conditions and child actions | That prompt's validated answer |
| `%flow_<name>%` | An Interaction Flow's confirmed child actions | The answer to a named field |
| `%player%`, `%player_name%` | Supported action text | The action target's name; a picker can substitute its selected name |
| `%executor%` | Command/picker text | The viewer who executes the action |
| `<player>`, `<player_name>`, `<executor>` | Command text | Alternate forms of the corresponding command aliases |

[Chat Fetcher](Chat-Fetcher.md) input belongs to its prompt. A nested prompt gets its own `%input%`. A following prompt captures the player-picker aliases that exist when it opens.

[Interaction Flows](Interaction-Flows.md) use names such as `%flow_choice%` and `%flow_amount%`. Their answers belong to that confirmed interaction; they are not temporary player-data fields or values passed into later parent actions.

Text aliases do not change the command sender, actual resource target or condition viewer.

## 🎁 Local Practical Reward Tokens

These tokens work in GUIPlus item names and lore without PlaceholderAPI:

| Token | Meaning |
|-------|---------|
| `%GUIPlus_reward_daily_total%` | Lifetime successful daily claims |
| `%GUIPlus_reward_daily_day%` | Current daily step |
| `%GUIPlus_reward_daily_remaining%` | Formatted wait remaining |
| `%GUIPlus_reward_daily_remaining_seconds%` | Wait remaining in seconds |
| `%GUIPlus_reward_daily_interval%` | Configured daily interval |
| `%GUIPlus_reward_playtime%` | Total playtime shown in minutes |
| `%GUIPlus_reward_starter_choice%` | Recorded starter choice |
| `%GUIPlus_reward_threshold_playtime-30m%` | Current requirement for that stable milestone ID |
| `%GUIPlus_reward_bundle_daily-1%` | Actual configured bundle for that reward ID |

Put a bundle token **alone on a lore line** to expand the bundle into separate readable lines:

```yaml
item-lore:
  - '&7Available after %GUIPlus_reward_daily_interval%'
  - '%GUIPlus_reward_bundle_daily-1%'
```

Use the corresponding stable reward ID for other bundles or milestone thresholds. These are local GUIPlus display tokens; they are not additional requests handled by the PlaceholderAPI expansion. See [Ready-to-Use Rewards](Ready-to-Use-Rewards.md).

## 🧩 State, Component and Catalog Parameters

| Token | Scope |
|-------|-------|
| `%state:key%` | Shared values inside one [Button State](Button-States.md) definition |
| `%component:key%` | Shared template defaults and instance values in [Shared Components](Shared-Components.md) |
| `%catalog_id%`, `%warp_name%`, `%page%`, `%pages%`, `%query%` | Managed [Warp Catalog](Warp-Catalogs.md) display text |

These are authored parameters, not global PlaceholderAPI values. Supported structured display and behavior text can use them; opaque item NBT is preserved rather than rewritten as text.

## ⏱ Cooldown Failure Text

`%cooldown_remaining_seconds%` is available in a cooldown denial's `conditionFailMessage` or the corresponding `messages.yml` text. It reads the first failed cooldown for the viewer, rounded up to seconds, without changing the timer.

```yaml
conditionFailMessage: '&cWait %cooldown_remaining_seconds% seconds.'
```

It is not a general timer placeholder for item lore. See [Conditions](Conditions.md#cooldown).

---

| ← Previous | Next → |
|:---|---:|
| [**Custom Heads & Skulls**](Custom-Heads-and-Skulls.md) | [**BungeeCord Support**](BungeeCord-Support.md) |
