> 🎨 **CREATING GUIS**

# Click Events

*Build buttons with commands, navigation, rewards and interactive steps*

---

An item's `click-events` map holds its actions in order. Add and edit them in the in-game editor, or use the YAML reference below.

## 📋 Quick Reference

| ID | Description |
|----|-------------|
| [`message`](#message) | Send a chat message |
| [`command`](#player-command) | Run commands as the viewer |
| [`console_command`](#console-command) | Run commands from console |
| [`title-click-event`](#title) | Show a title and subtitle |
| [`close-inventory`](#close-inventory) | Close the viewer's inventory |
| [`money-give`](#give-money) | Add Vault money |
| [`money-remove`](#remove-money) | Remove Vault money |
| [`money-set`](#set-money) | Set a Vault balance |
| [`bank-transfer`](#bank-transfer) | Deposit or withdraw a recorded bank balance |
| [`chat-fetcher`](#chat-fetcher) | Ask for validated chat input |
| [`interaction-flow`](#interaction-flow) | Collect answers, review and confirm |
| [`player-picker-by-console-command`](#player-picker-console-command) | Pick a player and run a console command |
| [`player-picker-by-player-command`](#player-picker-player-command) | Pick a player and run a viewer command |
| [`offline-player-picker-command`](#offline-player-picker) | Include offline players in a console-command picker |
| [`custom-item-give`](#custom-item-give) | Give a copied item stack |
| [`save-player-info`](#save-player-info) | Save per-player fields |
| [`practical-reward`](#practical-reward) | Claim a configured practical reward |
| [`take-items`](#take-item) | Remove matching inventory items |
| [`level-required`](#level-take) | Remove XP levels |
| [`xp-take`](#xp-take) | Remove experience points |
| [`open-gui`](#open-gui) | Open another GUI |
| [`back`](#open-last-gui) | Reopen a previous GUI |
| [`open-storage`](#open-storage) | Open personal persistent storage |
| [`sound-click-event`](#sound) | Play a sound |
| [`next-scene-click`](#next-scene) | Open the next scene |
| [`previous-scene-click`](#previous-scene) | Open the previous scene |
| [`glow-at-item`](#glow-at-item) | Add an enchantment glow on scene open |
| [`teleport`](#teleport) | Teleport the action's target |
| [`server-click-event`](#server-bungeecord) | Request a proxy server connection |
| [`player-points-remove`](#player-points-remove) | Remove PlayerPoints |

## 📐 Click Event Structure

Use the action ID as its key, or give the entry any unique key and specify `type` explicitly:

```yaml
click-events:
  message:
    message: '&aHello!'
  second-message:
    type: message
    message: '&7Right-click for more information.'
    clickType: RIGHT
```

Quote text beginning with `&` or `%`, and text containing `: `, so YAML reads it as text. Nested actions belong under another `click-events` map. Unknown action IDs or parameters are rejected; check the console for the file and reason.

## ⚙ Common Properties

| Property | Default | Description |
|----------|---------|-------------|
| `type` | Entry key | Registered action ID; required when the key is a custom name |
| `clickType` | `NONE` | `LEFT`, `RIGHT`, `MIDDLE`, `SHIFT_LEFT`, `SHIFT_RIGHT`, or `NONE` for any click |
| `executionDelay` | `0` | Delay in **ticks**; `20` is about one second |
| `waitForCompletion` | `true` | Wait for this action before continuing the parent list |
| `conditions` | Empty | Requirements checked for this action |
| `conditionFailMessage` | Default message | Text shown when its requirement fails |
| `click-events` | Empty | Child actions, used by inputs and confirmed interactions |

`executionDelay` accepts `0`–`86400`. The scheduler rounds it to whole ticks and waits at least one tick. With `waitForCompletion: false`, later actions may proceed while an input remains open. [Interaction Flows](Interaction-Flows.md) always wait for confirmation.

Conditions normally check the **viewer**. Vault money, copied-item rewards, costs and saved player fields affect the actual target, which defaults to the viewer. Practical rewards always use the viewer's own inventory and UUID progress. A player picker substitutes a selected name in text; it does not change the action target or the command sender. See [Commands & Permissions](Commands-and-Permissions.md) for targeted menu opening.

## 🖱 All Click Event Types

### Message
**ID:** `message`

Send a colored chat message to the viewer. Supports the current input, flow and player-name substitutions.

```yaml
click-events:
  message:
    message: '&aWelcome, %player%!'
```

### Player Command
**ID:** `command`

Run one or more commands as the viewer. Use `commands` as a list; singular `command` is also accepted for older definitions.

```yaml
click-events:
  command:
    commands:
      - 'spawn'
    setOp: false
```

`setOp: true` temporarily gives that viewer operator status for the command list. Use it only for commands you intend to run with full server authority.

In command text, `%player%`, `%player_name%`, `<player>` and `<player_name>` refer to the target; `%executor%` and `<executor>` refer to the viewer. After a picker, the player aliases contain the selected name.

### Console Command
**ID:** `console_command`

Run the list from the server console, with the same player aliases:

```yaml
click-events:
  console_command:
    commands:
      - 'say %executor% selected %player%'
```

A dispatched external command does not confirm that another plugin completed a purchase or granted ownership.

### Title
**ID:** `title-click-event`

Send a title to the target. `title-format` uses `title/?/subtitle/?/fadeIn/?/stay/?/fadeOut`; timings are ticks, defaulting to `20`, `60` and `20`.

```yaml
click-events:
  title-click-event:
    title-format: '&6Welcome!/?/&7Enjoy your stay/?/10/?/40/?/10'
```

Subtitle and timings may be omitted. This player title is separate from a GUI's inventory title format.

### Close Inventory
**ID:** `close-inventory`

Close the viewer's current inventory:

```yaml
click-events:
  close-inventory: {}
```

### Give Money
**ID:** `money-give`

Add Vault money to the target. Requires Vault and a working economy provider. `amount` can be a number or an input/flow substitution.

```yaml
click-events:
  money-give:
    amount: 100
```

### Remove Money
**ID:** `money-remove`

Remove Vault money from the target. Insufficient funds stop the action.

```yaml
click-events:
  money-remove:
    amount: '%input%'
```

Use bounded [Input Rules](Input-Rules.md) when accepting an amount from a player.

### Set Money
**ID:** `money-set`

Set the target's Vault balance to a fixed non-negative number. This field does not accept placeholders.

```yaml
click-events:
  money-set:
    amount: 1000
```

### Bank Transfer
**ID:** `bank-transfer`

Move money between the target's Vault account and one GUIPlus player-data field. Use `DEPOSIT` or `WITHDRAW`, a positive `amount`, and a bank `field` such as `deposited`.

```yaml
click-events:
  bank-transfer:
    direction: DEPOSIT
    amount: '%input%'
    field: deposited
```

Use this dedicated action for bank transfers. It records unresolved outcomes for staff review; do not replace it with independent payment and data-saving actions. See [Chat Fetcher](Chat-Fetcher.md#bank-deposit-example) and [Player Data](Player-Data.md#bank-balances).

### Chat Fetcher
**ID:** `chat-fetcher`

Prompt the viewer, validate their answer, then run child actions using `%input%`:

```yaml
click-events:
  chat-fetcher:
    message: '&eType a quantity from 1 to 64.'
    input-type: INTEGER
    input-min: 1
    input-max: 64
    input-max-length: 2
    click-events:
      message:
        message: '&aYou entered %input%.'
```

See [Chat Fetcher](Chat-Fetcher.md) and [Input Rules](Input-Rules.md).

### Interaction Flow
**ID:** `interaction-flow`

Collect named answers, show a review and run child actions only after Confirm. Configure fields and confirmed actions through the editor. Answers use `%flow_<name>%`, such as `%flow_choice%`.

```yaml
click-events:
  interaction-flow:
    flow-title: '&8Review your choice'
    flow-steps:
      '0':
        name: choice
        label: 'Choose a destination'
        input-type: ENUM
        input-max-length: 20
        input-options: [spawn, market]
    click-events:
      message:
        message: '&aYou confirmed %flow_choice%.'
```

See [Interaction Flows](Interaction-Flows.md) for Back, Cancel, field scope and confirmation requirements.

### Player Picker (Console Command)
**ID:** `player-picker-by-console-command`

Choose an online player, then run a console command using their name:

```yaml
click-events:
  player-picker-by-console-command:
    command: 'say %executor% selected %player%'
```

### Player Picker (Player Command)
**ID:** `player-picker-by-player-command`

Choose an online player, then run the command as the viewer:

```yaml
click-events:
  player-picker-by-player-command:
    command: 'msg %player% Hello from %executor%!'
```

The old ID `player-picker-command` is accepted as an alias for this action. A following chat prompt captures the current selected-name aliases when it opens.

### Offline Player Picker
**ID:** `offline-player-picker-command`

Include offline players in the selection list, then run the command from console:

```yaml
click-events:
  offline-player-picker-command:
    command: 'unban %player%'
```

### Custom Item Give
**ID:** `custom-item-give`

Give the target the stack stored in `item`. Choose or copy the actual stack through the editor to preserve its metadata and avoid hand-writing version-specific Bukkit item data.

### Save Player Info
**ID:** `save-player-info`

Save fields for the target with `field:value` pairs separated by commas:

```yaml
click-events:
  save-player-info:
    save-format: 'rank:literal:vip,points:number:100'
```

Use `literal:` for text, `number:` for numeric values and `calc:` for calculations. See [Player Data](Player-Data.md).

### Practical Reward
**ID:** `practical-reward`

Claim one canonical reward from the configured Daily, Playtime or Starter catalog. This action always claims for the viewer, even when the menu carries another action target:

```yaml
click-events:
  practical-reward:
    reward: daily-1
```

Claims remain attached to the player's UUID and stable reward ID across menu copies. See [Ready-to-Use Rewards](Ready-to-Use-Rewards.md) for all IDs, actual reward editing and inventory checks.

### Take Item
**ID:** `take-items`

Remove the stacks in the `items` list from the target. Configure the real stacks in the editor. `ignoreDurability: true` ignores damage when matching; it does not ignore other item metadata. This action uses camel-case `ignoreDurability`, whereas the `has-items` condition uses `ignore-durability`.

### Level Take
**ID:** `level-required`

Remove XP **levels** from the target; the action stops if they cannot pay the whole cost:

```yaml
click-events:
  level-required:
    level: 10
```

### XP Take
**ID:** `xp-take`

Remove experience **points**, rather than levels:

```yaml
click-events:
  xp-take:
    xp: 500
```

### Open GUI
**ID:** `open-gui`

Open the menu identified by `gui`. Its permission and scene admission requirements still apply. `open-gui` responds to item clicks and is skipped inside scene `open-events`, preventing automatic redirect loops.

```yaml
click-events:
  open-gui:
    gui: bank
```

### Open Last GUI
**ID:** `back`

Reopen a previous accessible player GUI from the viewer's history. This differs from `/gui !!`, which reopens the last editor screen.

```yaml
click-events:
  back: {}
```

### Open Storage
**ID:** `open-storage`

Open the viewer's personal pool, with size determined by the highest matching permission tier:

```yaml
click-events:
  open-storage:
    storage-id: vault
    title: '&8%player% Storage'
    rows: 3
    permission-rows:
      - 'guiplus.storage.vip:6'
```

Rows range from 1 to 6. Reusing `storage-id` reuses the same per-player contents. See [Configuration](Configuration.md).

### Sound
**ID:** `sound-click-event`

Play a Bukkit sound to the viewer, with optional `volume` and `pitch` defaulting to `1`:

```yaml
click-events:
  sound-click-event:
    sound: ENTITY_EXPERIENCE_ORB_PICKUP
    volume: 1.0
    pitch: 1.0
```

### Next Scene
**ID:** `next-scene-click`

```yaml
click-events:
  next-scene-click: {}
```

Open the next scene if its admission requirements pass. See [Scenes](Scenes.md).

### Previous Scene
**ID:** `previous-scene-click`

```yaml
click-events:
  previous-scene-click: {}
```

Open the previous scene if its admission requirements pass.

### Glow At Item
**ID:** `glow-at-item`

Add an enchantment glow when the item is displayed. This action automatically runs on open; its action conditions can decide which viewers see the glow.

```yaml
click-events:
  glow-at-item: {}
```

### Teleport
**ID:** `teleport`

Teleport the actual action target to a saved location. Set the destination in the editor, or use its labeled six-part format:

```yaml
click-events:
  teleport:
    location: 'World: world, X: 0.0, Y: 64.0, Z: 0.0, Yaw: 90.0, Pitch: 0.0'
```

The world must be available. A picker by itself only changes text aliases; it does not turn its selected name into this action's target.

### Server (BungeeCord)
**ID:** `server-click-event`

Send a BungeeCord `Connect` plugin message for the viewer:

```yaml
click-events:
  server-click-event:
    server: lobby
```

See [BungeeCord Support](BungeeCord-Support.md) for proxy setup.

### Player Points Remove
**ID:** `player-points-remove`

Remove a whole-number PlayerPoints amount from the target. This action is registered when PlayerPoints is available.

```yaml
click-events:
  player-points-remove:
    amount: 100
```

## 📂 Scene Open Events

A scene's `open-events` map runs actions when the scene is opened. See [Scenes](Scenes.md). `openEvent` is not a supported YAML action property; `glow-at-item` selects its open behavior automatically.

Use [Button States](Button-States.md) for Locked / Ready / Owned behavior, and [Shared Components](Shared-Components.md) to reuse an explicitly linked button. [Warp Catalogs](Warp-Catalogs.md) provide managed warp, page and search actions for automatic EssentialsX lists.

---

| ← Previous | Next → |
|:---|---:|
| [**Scenes**](Scenes.md) | [**Conditions**](Conditions.md) |
