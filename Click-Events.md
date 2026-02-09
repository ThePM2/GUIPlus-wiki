> 🎨 **CREATING GUIS**

# Click Events

*Actions that execute when a player clicks an item in a GUI*

---

Click events are actions that execute when a player clicks an item in a GUI. Each item can have multiple click events that execute in sequence.

## 📋 Quick Reference

| ID | Description |
|----|-------------|
| [`message`](#message) | Send a chat message |
| [`command`](#player-command) | Run command as the player |
| [`console_command`](#console-command) | Run command from console |
| [`title-click-event`](#title) | Show a title/subtitle |
| [`close-inventory`](#close-inventory) | Close the GUI |
| [`money-give`](#give-money) | Add Vault money |
| [`money-remove`](#remove-money) | Remove Vault money |
| [`money-set`](#set-money) | Set Vault balance |
| [`chat-fetcher`](#chat-fetcher) | Prompt for chat input |
| [`player-picker-by-console-command`](#player-picker-console-command) | Pick player, run console cmd |
| [`player-picker-by-player-command`](#player-picker-player-command) | Pick player, run player cmd |
| [`offline-player-picker-command`](#offline-player-picker) | Pick offline player, run cmd |
| [`custom-item-give`](#custom-item-give) | Give a custom item |
| [`save-player-info`](#save-player-info) | Save player data fields |
| [`take-items`](#take-item) | Remove items from inventory |
| [`level-required`](#level-take) | Remove XP levels |
| [`xp-take`](#xp-take) | Remove experience points |
| [`back`](#open-last-gui) | Reopen previous GUI |
| [`sound-click-event`](#sound) | Play a sound |
| [`next-scene-click`](#next-scene) | Go to next scene |
| [`previous-scene-click`](#previous-scene) | Go to previous scene |
| [`glow-at-item`](#glow-at-item) | Add enchant glow on scene open |
| [`teleport`](#teleport) | Teleport the target player |
| [`server-click-event`](#server-bungeecord) | Send to another server |
| [`player-points-remove`](#player-points-remove) | Remove PlayerPoints |

## 📐 Click Event Structure

Click events are defined inside an item's `click-events` map:

```yaml
click-events:
  message:
    message: &aHello!
  command:
    commands:
      - say Hello from %player%
```

## ⚙ Common Properties

Every click event supports these YAML properties:

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `clickType` | String | `NONE` | Which mouse button triggers this event: `LEFT`, `RIGHT`, `MIDDLE`, `SHIFT_LEFT`, `SHIFT_RIGHT`, `NONE` (any click) |
| `executionDelay` | Number | `0` | Delay in **ticks** before this event executes (20 ticks = 1 second) |

> **Note:** `executionDelay` is measured in ticks, not seconds. Use `20` for a 1-second delay, `40` for 2 seconds, etc.

The in-game editor also supports `waitForCompletion` (whether subsequent events wait for this one to finish) and `openEvent` (run on scene open instead of click). These are stored in the item's compressed data by the editor but are not directly configurable via hand-written YAML.

Example with click type restriction:

```yaml
click-events:
  message:
    message: &aYou left-clicked!
    clickType: LEFT
```

## 🖱 All Click Event Types

### Message
**ID:** `message`

Sends a colored chat message to the player. Supports PlaceholderAPI placeholders.

```yaml
click-events:
  message:
    message: &aWelcome to the server, %player%!
```

---

### Player Command
**ID:** `command`

Executes one or more commands as the clicking player.

```yaml
click-events:
  command:
    commands:
      - spawn
```

The YAML field is `commands` (plural, list). A singular `command` key works for backwards compatibility.

| Property | Type | Description |
|----------|------|-------------|
| `commands` | List | Commands to execute as the player |
| `setOp` | Boolean | When `true`, temporarily grants OP status to execute the command (default: `false`) |

| Placeholder | Description |
|-------------|-------------|
| `%player%` | The clicking player's name |
| `%executor%` | The player who executed the action |

```yaml
# Run multiple commands with temporary OP
click-events:
  command:
    commands:
      - spawn
      - heal
    setOp: true
```

> **Warning:** Use `setOp: true` with caution. It temporarily grants the player full operator permissions to execute these commands.

---

### Console Command
**ID:** `console_command`

Executes one or more commands from the server console.

```yaml
click-events:
  console_command:
    commands:
      - give %player% diamond 1
```

Same as Player Command — uses `commands` (plural, list).

```yaml
# Run multiple console commands
click-events:
  console_command:
    commands:
      - give %player% diamond 1
      - broadcast %player% received a diamond!
```

---

### Title
**ID:** `title-click-event`

Sends a title and subtitle to the player with configurable fade times.

**Format:** `title/?/subtitle/?/fadeIn/?/stay/?/fadeOut`

```yaml
click-events:
  title-click-event:
    title-format: &6Welcome/?/&7to our server/?/10/?/40/?/10
```

The YAML field is `title-format` (not `title`).

| Parameter | Position | Default | Description |
|-----------|----------|---------|-------------|
| title | 1st | — | The main title text (required) |
| subtitle | 2nd | empty | The subtitle text |
| fadeIn | 3rd | `20` | Fade-in time in ticks |
| stay | 4th | `60` | Stay time in ticks |
| fadeOut | 5th | `20` | Fade-out time in ticks |

Separate each parameter with `/?/`. Only the title is required — all other parameters are optional and use defaults if omitted.

```yaml
# Minimal - title only, uses default timing
click-events:
  title-click-event:
    title-format: &6Welcome!

# With subtitle
click-events:
  title-click-event:
    title-format: &6Welcome!/?/&7to our server
```

---

### Close Inventory
**ID:** `close-inventory`

Closes the player's currently open inventory.

```yaml
click-events:
  close-inventory: {}
```

---

### Give Money
**ID:** `money-give`

Adds money to the player's account. Requires Vault.

```yaml
click-events:
  money-give:
    amount: 1000
```

The `amount` field supports PlaceholderAPI placeholders, e.g. `amount: '%input%'`.

---

### Remove Money
**ID:** `money-remove`

Subtracts money from the player's account. Requires Vault.

```yaml
click-events:
  money-remove:
    amount: 500
```

---

### Set Money
**ID:** `money-set`

Sets the player's account balance to an exact amount. Requires Vault.

```yaml
click-events:
  money-set:
    amount: 0
```

> **Note:** Unlike `money-give` and `money-remove`, the `amount` field in `money-set` is a fixed number and does **not** support PlaceholderAPI placeholders.

---

### Chat Fetcher
**ID:** `chat-fetcher`

Prompts the player to type input in chat. After input is received, nested click events are executed with access to the `%input%` placeholder.

```yaml
click-events:
  chat-fetcher:
    message: &ePlease type a number in chat!
    conditionFailMessage: &cInvalid input!
    conditions:
      is-integer:
        value: '%input%'
    click-events:
      message:
        message: &aYou typed %input%!
```

See [Chat Fetcher](Chat-Fetcher.md) for full details.

---

### Player Picker (Console Command)
**ID:** `player-picker-by-console-command`

Opens an online player selection GUI. When a player is selected, executes a console command.

```yaml
click-events:
  player-picker-by-console-command:
    command: give %player% diamond 1
```

`%player%` is replaced with the selected player's name.

---

### Player Picker (Player Command)
**ID:** `player-picker-by-player-command`

Opens an online player selection GUI. When a player is selected, the clicking player executes a command.

```yaml
click-events:
  player-picker-by-player-command:
    command: msg %player% Hello from %executor%!
```

---

### Offline Player Picker
**ID:** `offline-player-picker-command`

Opens a player selection GUI that includes offline players. Runs as a console command.

```yaml
click-events:
  offline-player-picker-command:
    command: unban %player%
```

---

### Custom Item Give
**ID:** `custom-item-give`

Gives a custom configured item to the player. This click event stores items as serialized Bukkit ItemStack objects.

> **Tip:** This is best configured through the in-game editor rather than by hand. The editor lets you place any item and it will be serialized automatically.

```yaml
# Configured via the in-game editor, stored as a serialized ItemStack:
click-events:
  custom-item-give:
    item:
      ==: org.bukkit.inventory.ItemStack
      v: 3700
      type: DIAMOND_SWORD
      amount: 1
```

---

### Save Player Info
**ID:** `save-player-info`

Saves data fields to the player's persistent data. Supports math operations.

**Format:** `field1:value1,field2:value2`

```yaml
click-events:
  save-player-info:
    save-format: kills:%GUIPlus_player_info_kills%+1
```

Math operators supported: `+`, `-`, `*`, `/`, `^` (exponentiation), and functions `sqrt()`, `sin()`, `cos()`, `tan()`. Parentheses are supported for grouping.

See [Player Data](Player-Data.md) for full details.

---

### Take Item
**ID:** `take-items`

Removes specific items from the player's inventory. Items are stored as serialized Bukkit ItemStack objects.

> **Tip:** This is best configured through the in-game editor. The editor lets you select which items to remove.

```yaml
# Configured via the in-game editor, stored as serialized ItemStacks:
click-events:
  take-items:
    items:
      - ==: org.bukkit.inventory.ItemStack
        v: 3700
        type: DIAMOND
        amount: 5
    ignoreDurability: false
```

| Property | Type | Description |
|----------|------|-------------|
| `items` | List | List of serialized Bukkit ItemStack objects to remove |
| `ignoreDurability` | Boolean | When `true`, ignores item durability when matching |

---

### Level Take
**ID:** `level-required`

Removes XP levels from the player.

```yaml
click-events:
  level-required:
    level: 10
```

The YAML field is `level` (singular, not `levels`).

---

### XP Take
**ID:** `xp-take`

Removes experience points (not levels) from the player.

```yaml
click-events:
  xp-take:
    xp: 500
```

---

### Open Last GUI
**ID:** `back`

Reopens the player's previously viewed GUI.

```yaml
click-events:
  back: {}
```

---

### Sound
**ID:** `sound-click-event`

Plays a sound at the player's location.

```yaml
click-events:
  sound-click-event:
    sound: ENTITY_EXPERIENCE_ORB_PICKUP
    volume: 1.0
    pitch: 1.0
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `sound` | String | — | The Bukkit sound name |
| `volume` | Float | `1.0` | Volume of the sound |
| `pitch` | Float | `1.0` | Pitch of the sound |

You can find all valid sound names in the [Bukkit Sound enum](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/Sound.html).

---

### Next Scene
**ID:** `next-scene-click`

Navigates to the next scene in the GUI.

```yaml
click-events:
  next-scene-click: {}
```

See [Scenes](Scenes.md) for multi-page navigation.

---

### Previous Scene
**ID:** `previous-scene-click`

Navigates to the previous scene in the GUI.

```yaml
click-events:
  previous-scene-click: {}
```

---

### Glow At Item
**ID:** `glow-at-item`

Adds an enchanted glow effect to the item when the scene opens. This is an **open event** — it runs automatically when the scene is displayed, not when the player clicks. The glow is tracked per-player, so each player sees it independently.

> **Note:** This event is configured through the in-game editor. It automatically sets `openEvent: true` internally and can be combined with conditions to glow only for certain players.

```yaml
click-events:
  glow-at-item: {}
```

---

### Teleport
**ID:** `teleport`

Teleports the target player to a specific location. In normal use the target is the clicking player, but when used with a player picker, the selected player is teleported instead.

**Supported formats:**

| Format | Example | Description |
|--------|---------|-------------|
| `world,x,y,z,yaw,pitch` | `world,0,100,0,90,0` | Full location with rotation |
| `world,x,y,z` | `world,0,100,0` | Location without rotation |
| `x,y,z,yaw,pitch` | `0,100,0,90,0` | Coordinates in player's current world |
| `x,y,z` | `0,100,0` | Coordinates in player's current world |
| `world` | `world_nether` | Teleport to a world's spawn point |
| `current` | `current` | Current location (useful with PlaceholderAPI) |

```yaml
click-events:
  teleport:
    location: world,0,100,0,0,0
```

> **Tip:** Use `current` with PlaceholderAPI to create dynamic teleport destinations, or use a world name to send players to a world's spawn.

---

### Server (BungeeCord)
**ID:** `server-click-event`

Sends the player to another server on your BungeeCord/Velocity network.

```yaml
click-events:
  server-click-event:
    server: lobby
```

See [BungeeCord Support](BungeeCord-Support.md).

---

### Player Points Remove
**ID:** `player-points-remove`

Subtracts PlayerPoints currency from the player. Requires the PlayerPoints plugin.

```yaml
click-events:
  player-points-remove:
    amount: 100
```

## 🔗 Combining Multiple Click Events

Items can have multiple click events that execute in sequence:

```yaml
click-events:
  sound-click-event:
    sound: ENTITY_PLAYER_LEVELUP
  message:
    message: &6Congratulations!
  console_command:
    commands:
      - give %player% diamond 5
  close-inventory: {}
```

## 🎯 Click Type Restrictions

Restrict events to specific mouse buttons by assigning unique keys:

```yaml
click-events:
  buy-message:
    message: &aYou bought the item!
    clickType: LEFT
  info-message:
    message: &7Item info here...
    clickType: RIGHT
```

> **Note:** Each click event entry needs a unique key in YAML. If you need two events of the same type (e.g., two `message` events), give them different keys like `buy-message` and `info-message`. The event type is determined by the internal structure, not the key name.

## 📂 Open Events

Some click events can run when the scene opens instead of on click. The `glow-at-item` event does this automatically. For other events, `openEvent` can be configured through the in-game editor — it is stored in the item's compressed data rather than as a direct YAML field.

Scenes also support their own `open-events` section — see [Scenes](Scenes.md) for details.

---

| ← Previous | Next → |
|:---|---:|
| [**Scenes**](Scenes.md) | [**Conditions**](Conditions.md) |
