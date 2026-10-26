> 💬 **INTERACTIVE**

# Interaction Flows

*Collect named answers, show a review and run actions after confirmation*

---

An Interaction Flow guides the player through choices or validated chat fields, then shows their answers for review. Use it for a starter selection, a quantity prompt or another action that needs an explicit **Confirm** step.

## 🛠 Build a Flow In-Game

1. Edit a GUI item and add an **Interaction Flow** click event.
2. Choose **Add field**, then set its **Field name** and **Player prompt**.
3. Open **Input rules / choices** to select an input type, length, numeric bounds or choices.
4. Use **Move earlier** and **Move later** to set the order.
5. Set the **Summary title** and add the effects under **Confirmed actions**.
6. Choose **Apply flow**, finish the action setup with the normal **Back** path, then save the GUI.

The editor keeps a detached draft. **Cancel draft** or closing the editor discards its changes. Applying a flow updates its action setup; the GUI still needs to be saved.

> **Try an example:** `/gui examples choices my_choices` installs an independent menu with clickable choices, a bounded quantity and a confirmation message. It requires `guiplus.gui.examples` and the configured `/gui` root permission.

## 👤 The Player's Steps

1. **Choose or enter:** ENUM fields appear as clickable options. Other types use validated chat entry.
2. **Review:** The player sees all their answers before any confirmed actions run.
3. **Edit:** Back or clicking an answer returns to an earlier field. Accepting that answer again clears dependent later answers, so those steps must be completed again.
4. **Confirm:** The configured confirmed actions run once.

Cancel or closing the interaction aborts it. During a chat field, typing `exit` returns to that field's screen without accepting an answer. Disconnecting, reloading or changing the active definition invalidates the interaction.

## 🏷 Named Values

Each field provides a token named `%flow_<name>%`. A field named `amount` supplies `%flow_amount%`; a field named `choice` supplies `%flow_choice%`.

Values belong to that interaction and its confirmed child actions. They are not automatically saved in [Player Data](Player-Data.md), and later sibling actions outside the flow do not inherit them. A nested flow starts its own named-value scope.

Field names must be unique, use **1–32 lowercase letters, digits or `_`**, and start with a letter. A flow supports **1–16 fields**. Summary titles and player prompts use **1–128 characters** without control characters.

## 📐 Example: Choose a Style and Quantity

Place this fragment on an existing item's `click-events`. It collects two answers and sends a message after Confirm; it does not grant a bundle.

```yaml
click-events:
  '0':
    type: interaction-flow
    flow-title: '&8Review your bundle'
    flow-steps:
      '0':
        name: 'style'
        label: 'Choose a bundle style'
        input-type: 'ENUM'
        input-max-length: 16
        input-options:
          - 'Explorer'
          - 'Builder'
          - 'Farmer'
      '1':
        name: 'quantity'
        label: 'Enter a quantity from 1 to 64'
        input-type: 'INTEGER'
        input-max-length: 2
        input-min: '1'
        input-max: '64'
    click-events:
      '0':
        type: message
        message: '&aConfirmed: %flow_quantity%x %flow_style%.'
```

`flow-steps` is a mapping numbered consecutively from `'0'`. Its child `click-events` are the **Confirmed actions**. See [Input Rules](Input-Rules.md) for every input type and choice limits.

## ✅ Requirements and Action Order

Flow-level `conditions` must use supported read-only built-in checks because they are checked again on confirmation. The editor filters unsupported flow requirements. Put consuming cooldown conditions on confirmed child actions.

Keep every effect that needs confirmation inside **Confirmed actions**. Earlier actions in the item's action list have already run when the flow opens. A flow always waits for confirmation; `waitForCompletion: false` is rejected.

Confirmation controls when the child actions start. Arbitrary commands and changes made by other plugins still follow those plugins' own success and failure rules.

---

| ← Previous | Next → |
|:---|---:|
| [**Button States**](Button-States.md) | [**Input Rules**](Input-Rules.md) |
