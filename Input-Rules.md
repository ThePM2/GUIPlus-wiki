> 💬 **INTERACTIVE**

# Input Rules

*Validate player input before it reaches your actions*

---

Input rules define what a player may enter in a [Chat Fetcher](Chat-Fetcher.md) or [Interaction Flow](Interaction-Flows.md). Choose a suitable type, set a maximum length and add numeric bounds or fixed choices where needed.

## 🛠 Edit Rules In-Game

For **Chat Fetcher**, open its action settings and choose **Input rules**. On an older customized layout, **Shift-right-click the prompt-message control** opens the same editor.

For an **Interaction Flow**, edit a field and choose **Input rules / choices**.

1. Select the input type.
2. Set **Maximum Length**.
3. For INTEGER or DECIMAL, set **Minimum** and **Maximum**. Right-click either bound to make it unbounded; entering `clear` does the same.
4. For ENUM, enter choices separated by `|`, for example `Builder|Explorer|Farmer`.
5. Choose **Apply**, finish the containing action or flow, then save the GUI.

Cancel or closing the rules editor discards its draft. Apply changes the rules on the action being edited; it does not append a new action or save the GUI by itself.

## 📋 Available Types

| Type | Accepted value |
|---|---|
| `TEXT` | Text within the length limit. Leading and trailing spaces are preserved. |
| `INTEGER` | A whole number such as `12` or `-3`. Optional leading `+` is accepted. |
| `DECIMAL` | A plain decimal such as `12.5` or `.5`. Exponent notation such as `1e3` is rejected. |
| `IDENTIFIER` | One value starting with a letter, digit or `_`; remaining characters may also include `.`, `:`, or `-`. |
| `PLAYER_NAME` | 1–16 letters, digits or underscores. This checks the format; it does not find or select an online player. |
| `ENUM` | One of your configured choices, matched without case sensitivity and returned using its configured spelling. |

All types reject control characters. Maximum length can be **1–16,384 characters**. Non-TEXT input is trimmed after the length check. Numeric bounds are inclusive, and the minimum cannot exceed the maximum. Accepted numbers are normalized: for example, decimal `2.00` becomes `2`.

Choose INTEGER or DECIMAL for amounts, PLAYER_NAME for a name and ENUM for a fixed selection. TEXT can contain spaces and command syntax, so use a narrower type when the value will become a command argument.

## 📐 Example: A Bounded Amount

Place this fragment on an existing item's `click-events`. The player must enter a whole number from 1 to 64 before the nested message runs.

```yaml
click-events:
  '0':
    type: chat-fetcher
    message: '&eEnter an amount from 1 to 64:'
    input-type: 'INTEGER'
    input-max-length: 2
    input-min: '1'
    input-max: '64'
    click-events:
      '0':
        type: message
        message: '&aYou entered %input%.'
```

Chat Fetcher exposes the accepted value as `%input%` to its nested actions and checks. Flow fields expose their named `%flow_<name>%` tokens instead.

## 🧭 Fixed Choices

For a Chat Fetcher action, keep its normal `message` and replace its input-rule fields with:

```yaml
input-type: 'ENUM'
input-max-length: 16
input-options:
  - 'Builder'
  - 'Explorer'
  - 'Farmer'
```

The same fields can be placed inside one `flow-steps.<number>` definition. Chat Fetcher accepts the choice in chat; a flow displays clickable options.

The in-game choice editor supports **up to 64 choices**, each **1–128 characters**. Flow definitions enforce the same limits. Choices must be distinct without case sensitivity, fit the maximum input length and cannot use the reserved word `exit`. Use a YAML list when a choice contains `|`, since that character separates entries in the chat editor.

## ✅ Additional Conditions

Built-in input rules validate the value's shape and bounds. Existing action conditions can then check requirements such as the player's balance. A valid number does not by itself establish that a player can afford an action or has permission to run it.

Use [Interaction Flows](Interaction-Flows.md) when the player should review several answers before confirming effects.

---

| ← Previous | Next → |
|:---|---:|
| [**Interaction Flows**](Interaction-Flows.md) | [**Ready-to-Use Rewards**](Ready-to-Use-Rewards.md) |
