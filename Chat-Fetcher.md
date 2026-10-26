> 💬 **INTERACTIVE**

# Chat Fetcher

*Ask for one answer, validate it and continue your button's actions*

---

A `chat-fetcher` action closes the viewer's menu and asks them to type in chat. Accepted input becomes `%input%` in that prompt's conditions and child actions. For several named answers with a review and Confirm button, use [Interaction Flows](Interaction-Flows.md).

## ⚙ How It Works

1. Add **Chat Fetcher** to an item's click actions.
2. Set the prompt message and open **Input Rules** to choose the input type and limits.
3. Add any extra conditions, then the actions that use the answer.
4. Return with Back and save the GUI.
5. The player answers in chat; a rejected answer keeps the prompt open.

The Input Rules editor uses Apply / Cancel. Changing the rule updates the draft; save the ordinary GUI after returning from the action editor. See [Input Rules](Input-Rules.md) for the complete editor controls.

## 📐 Basic Structure

```yaml
click-events:
  chat-fetcher:
    message: '&eWhat would you like to call this?'
    input-type: TEXT
    input-max-length: 40
    click-events:
      message:
        message: '&aYou entered: %input%'
```

The action ID can be its map key. A custom key needs `type: chat-fetcher`, just like other [Click Events](Click-Events.md).

## ✅ Input Types and Limits

| Property | Default | Purpose |
|----------|---------|---------|
| `input-type` | `TEXT` | `TEXT`, `INTEGER`, `DECIMAL`, `IDENTIFIER`, `PLAYER_NAME` or `ENUM` |
| `input-max-length` | `2048` | Maximum answer length, from 1 to 16384 characters |
| `input-min` | Unbounded | Minimum numeric value |
| `input-max` | Unbounded | Maximum numeric value |
| `input-options` | Empty | Accepted choices for ENUM |
| `disable-exit-message` | `true` | Suppress the normal exit reminder for this prompt |
| `conditionFailMessage` | Default denial | Feedback when additional conditions fail |

TEXT keeps the entered text, including spaces. Other types trim surrounding spaces. INTEGER and DECIMAL accept plain numbers, reject exponent notation and non-finite values, and normalize accepted numbers. All types reject control characters. ENUM matches choices without case sensitivity and returns the configured spelling.

```yaml
click-events:
  chat-fetcher:
    message: '&eEnter a whole number from 1 to 100.'
    input-type: INTEGER
    input-min: 1
    input-max: 100
    input-max-length: 3
    click-events:
      message:
        message: '&aQuantity: %input%'
```

For an identifier, choose IDENTIFIER; PLAYER_NAME checks the syntax of a Minecraft username without proving that player exists. ENUM needs a nonempty list:

```yaml
input-type: ENUM
input-max-length: 20
input-options: [Builder, Explorer, Farmer]
```

In a Chat Fetcher, the player types an ENUM choice in chat. [Interaction Flows](Interaction-Flows.md) present ENUM options as clickable choices.

## 🔍 Additional Conditions

Input rules validate the type and range first. `conditions` then checks any extra requirements for the **viewer**, with access to the accepted `%input%`.

```yaml
click-events:
  chat-fetcher:
    message: '&eEnter an amount you can afford.'
    input-type: DECIMAL
    input-min: 0.01
    input-max: 1000000
    input-max-length: 12
    conditionFailMessage: '&cYou do not have that much money.'
    conditions:
      has-money:
        required-balance: '%input%'
    click-events:
      message:
        message: '&aYour answer %input% passed the balance check.'
```

Conditions do not remove money. Configure the desired effect separately. See [Conditions](Conditions.md).

## 🏦 Bank Deposit Example

Use the dedicated `bank-transfer` action to update both the Vault account and the GUIPlus bank ledger:

```yaml
click-events:
  chat-fetcher:
    message: '&eHow much would you like to deposit?'
    input-type: DECIMAL
    input-min: 0.01
    input-max: 1000000
    input-max-length: 12
    click-events:
      bank-transfer:
        direction: DEPOSIT
        field: deposited
        amount: '%input%'
      message:
        message: '&aDeposited %input%.'
      open-gui:
        gui: bank
```

Use `WITHDRAW` for the reverse direction. The target defaults to the viewer; keep financial menus untargeted unless you deliberately intend to affect another player. A transfer with an uncertain provider outcome stops for reconciliation. Staff can inspect it with `/guiplus-transactions list` and `show <id>` before confirming the result from provider records. See [Player Data](Player-Data.md#bank-balances).

## 🏷 Title Mode Messages

A message containing `!@!` appears as a player title rather than chat text:

```yaml
message: '&6Enter Amount!@!&7Type a number in chat!@!20!@!60!@!20'
```

The format is `title!@!subtitle!@!fadeIn!@!stay!@!fadeOut`. Optional timings are ticks and default to `20`, `60` and `20`.

## ⏳ Waiting and Cancelling

By default, `waitForCompletion: true` waits for accepted input and its child actions to finish before continuing the parent list. An invalid answer starts no child actions. Typing `exit` abandons the prompt; the reserved keyword cannot be used as a normal answer.

With `waitForCompletion: false`, later parent actions can run as soon as the prompt opens. Cancelling that prompt does not reverse an already completed click. Use the default waiting behavior when the following work depends on the answer.

Each nested chat prompt has its own `%input%`. A player-picker selection is captured when the prompt opens, so supported player-name aliases can continue into its child actions. This preserves text substitutions; it does not change who sends commands, receives resource actions or has conditions checked.

Reloading, deleting a definition or leaving the server cancels pending work. For a linked [Shared Component](Shared-Components.md), a changed component also retires the old prompt.

## ⚙ Server Configuration

| `config.yml` option | Default | Description |
|---------------------|---------|-------------|
| `chat-fetcher-exit-message-cooldown` | `10` | Seconds between exit reminders when enabled |
| `disable-commands-chat-fetcher` | `true` | Block commands while waiting for input |
| `disable-guis-chat-fetcher` | `true` | Block ordinary GUI opening while waiting for input |

---

| ← Previous | Next → |
|:---|---:|
| [**Conditions**](Conditions.md) | [**Player Data**](Player-Data.md) |
