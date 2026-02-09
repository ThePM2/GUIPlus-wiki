> 💬 **INTERACTIVE**

# Chat Fetcher

*Prompt players for text input and use it in click events*

---

The Chat Fetcher system allows you to prompt players for text input via chat and use that input in subsequent click events. This is one of the most powerful features in GUIPlus, enabling interactive forms, search menus, and dynamic actions.

## ⚙ How It Works

1. Player clicks an item with a `chat-fetcher` click event
2. The GUI closes and the player sees a prompt message in chat
3. The player types their response in chat
4. GUIPlus validates the input against any conditions
5. If validation passes, nested click events execute with the `%input%` placeholder replaced by what the player typed
6. If validation fails, the `conditionFailMessage` is shown

## 📐 Basic Structure

```yaml
click-events:
  chat-fetcher:
    message: §eType something in chat!
    click-events:
      message:
        message: §aYou typed: %input%
```

## ✅ With Input Validation

Add conditions to validate the player's input before executing events:

```yaml
click-events:
  chat-fetcher:
    message: §eType a number between 1 and 100:
    conditionFailMessage: §cPlease enter a valid number!
    conditions:
      is-integer:
        value: '%input%'
    click-events:
      message:
        message: §aYou entered the number %input%!
```

## The `%input%` Placeholder

Inside a chat fetcher's nested `click-events` and `conditions`, the special `%input%` placeholder is available. It contains exactly what the player typed in chat.

```yaml
click-events:
  chat-fetcher:
    message: §eHow much do you want to deposit?
    conditionFailMessage: §cInvalid amount or insufficient funds!
    conditions:
      is-integer:
        value: '%input%'
      has-money:
        required-balance: '%input%'
    click-events:
      money-remove:
        amount: '%input%'
      save-player-info:
        save-format: deposited:%input%+%GUIPlus_player_info_deposited%
      message:
        message: §6You have deposited %input% to the bank!
      command:
        commands:
          - gui open bank %player%
```

## ⚙ Configuration Options

Several `config.yml` options control chat fetcher behavior:

| Option | Default | Description |
|--------|---------|-------------|
| `chat-fetcher-exit-message-cooldown` | `10` | Seconds between re-showing the "type `exit` to cancel" message |
| `disable-commands-chat-fetcher` | `true` | Block commands while waiting for input |
| `disable-guis-chat-fetcher` | `true` | Block GUI opening while waiting for input |

## 🏷 Title Mode Messages

If the chat fetcher `message` contains `!@!`, it is displayed as a title/subtitle on the player's screen instead of a chat message:

```yaml
click-events:
  chat-fetcher:
    message: §6Enter Amount!@!§7Type a number in chat!@!20!@!60!@!20
    click-events:
      message:
        message: §aYou typed: %input%
```

The `!@!` format is: `title!@!subtitle!@!fadeIn!@!stay!@!fadeOut`

| Part | Default | Description |
|------|---------|-------------|
| title | — | Main title text (required) |
| subtitle | empty | Subtitle text |
| fadeIn | `20` | Fade-in time in ticks |
| stay | `60` | Stay time in ticks |
| fadeOut | `20` | Fade-out time in ticks |

If the message does **not** contain `!@!`, it is sent as a normal chat message.

## ❌ Cancelling Input

Players can cancel the chat fetcher prompt by typing `exit` in chat. They will be returned to normal mode.

> **Note:** The `exit` keyword is built-in and cannot be changed. The "type `exit` to cancel" reminder message frequency is controlled by the `chat-fetcher-exit-message-cooldown` config option.

## 🏦 Full Example: Banking System

This example creates a deposit feature where players type an amount, it validates the input, removes the money, saves it as a bank balance, and reopens the bank GUI:

```yaml
'1':
  slot: 12
  item: PLAYER_HEAD
  amount: 1
  item-name: §e§lDeposit Money
  item-lore:
    - ''
    - §7Want to secure your money?
    - §7You have come to the right place!
    - ''
    - §bClick to deposit
  skullBase64: eyJ0ZXh0dXJlcy...
  click-events:
    chat-fetcher:
      message: §ePlease type the amount to deposit in chat! §8(Make sure you have enough money)
      conditionFailMessage: §cYou either don't have enough money or typed a non-number value!
      conditions:
        is-integer:
          value: '%input%'
        has-money:
          required-balance: '%input%'
      click-events:
        money-remove:
          amount: '%input%'
        save-player-info:
          save-format: deposited:%input%+%GUIPlus_player_info_deposited%
        message:
          message: §6%player% you have successfully deposited %input% to the bank!
        command:
          commands:
            - gui open bank %player%
```

---

| ← Previous | Next → |
|:---|---:|
| [**Conditions**](Conditions) | [**Player Data**](Player-Data) |
