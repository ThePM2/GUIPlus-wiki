> ❓ **HELP**

# FAQ & Troubleshooting

*Answers to common setup and menu questions.*

---

## 💡 Frequently Asked Questions

### Where should I start?

Run `/rewards` to try the included practical menus, or `/gui` to create your own. Existing installations can add the missing practical menus with `/gui practical install`. See [Getting Started](Getting-Started.md).

### Do the daily rewards require a login streak?

No. Players claim in the menu. The first daily reward is ready immediately; the default wait is 24 hours after a successful claim. Missing a day preserves progress. See [Ready-to-Use Rewards](Ready-to-Use-Rewards.md).

### How do I change what the player actually receives?

Use `/gui practical edit` for the included rewards. Changing the item in a menu changes its appearance, not the real prize bundle. **Apply & save all rewards** saves the catalog; existing claims and running daily deadlines are preserved.

### How do I change the same button in several menus?

Create a template with `/gui components`, then explicitly link the relevant items. Set local name or lore overrides where needed. After applying a shared change, reopen the affected menus. See [Shared Components](Shared-Components.md).

### How do I create a Locked or Owned button?

Use **Button States** in the item editor. Owned and Ready need visibility requirements; Locked is the fallback. For a starting point, install `/gui examples progression my_progress`. See [Button States](Button-States.md).

### Can I ask for several answers before running a command?

Add an **Interaction Flow**. Fields can use chat input or clickable choices. Put effects that need confirmation under **Confirmed actions**. Back edits an answer; Cancel grants nothing. See [Interaction Flows](Interaction-Flows.md).

### How do I keep player input within a range?

Use [Input Rules](Input-Rules.md) on a chat fetcher or flow field. For example, choose `INTEGER` with `input-min: 1` and `input-max: 64`. A numeric format check alone does not impose a positive range.

### How do I make an item visible only to VIPs?

Use `showConditions` on the item. Add the same permission to `conditions` when it must also protect execution:

```yaml
showConditions:
  has-permission:
    permission: myserver.vip
conditions:
  has-permission:
    permission: myserver.vip
conditionFailMessage: '&cVIP only!'
```

For an entire menu use its root `permission`; for a scene use `open-conditions`. See [Conditions](Conditions.md).

### How do I make a multi-page menu?

Use [Scenes](Scenes.md) with `disable-scene-animation: true` and Next/Previous buttons. Leave that setting `false` for a timed sequence instead. A scene's delay holds it before the next automatic transition; it is not a delay before the first page opens.

### Can another plugin or an NPC open a menu?

Yes. Configure it to run `gui open <id> <player>` from console, using that plugin's player-name placeholder. The menu's permissions and scene entry checks still apply to the viewer. See [Plugin Integrations](Plugin-Integrations.md).

### How do I open a menu for another player?

`/gui open shop Alex` opens the shop on Alex's screen. `/gui openTargeted staff_panel Alex` keeps the menu on your own screen with Alex as its action target. They have different permission checks. See [Commands & Permissions](Commands-and-Permissions.md).

### Does the warp catalog replace EssentialsX?

No. EssentialsX supplies the warp names and handles permissions, fees and teleport timing. GUIPlus generates the catalog, pages and search. Use `/gui warps` to edit it and `/guiplus-warps` to browse. See [Warp Catalogs](Warp-Catalogs.md).

### Which placeholders need PlaceholderAPI?

Other plugins' placeholders need PlaceholderAPI and the relevant expansions. Ordinary display of `%GUIPlus_player_info_<field>%` also uses its expansion. Built-in practical reward tokens work without it, as do flow/state/component parameters within their own scope. See [PlaceholderAPI Placeholders](PlaceholderAPI-Placeholders.md).

### How do I show the cooldown time left?

Use `%cooldown_remaining_seconds%` in the item's or action's denial message:

```yaml
conditions:
  cooldown:
    cooldown: 60000
    id: information_button
conditionFailMessage: '&cTry again in %cooldown_remaining_seconds%s.'
```

The duration above is in milliseconds. The remaining value is rounded up to whole seconds and comes from the cooldown that actually failed. See [Conditions](Conditions.md).

### Can I use MiniMessage or a custom title font?

Use `title-format: MINIMESSAGE` on Paper. A custom font also needs the matching client resource pack. The default title mode is LEGACY; changing it does not convert item lore or every message to rich text. See [Creating GUIs](Creating-GUIs.md#minimessage-inventory-titles).

### Can I use custom items or proxy servers?

Copy a held item's supported metadata in the editor, or configure another plugin's give command. Custom textures need that plugin's resource pack. Proxy transfers use the BungeeCord plugin-messaging channel; see [Custom Heads & Skulls](Custom-Heads-and-Skulls.md), [Plugin Integrations](Plugin-Integrations.md) and [BungeeCord Support](BungeeCord-Support.md).

## 🔧 Troubleshooting

### GUI does not open

- Check the console after `/gui reload` and make sure the requested menu ID exists.
- Check the command permission, menu permission and scene `open-conditions` separately.
- Use `/gui open <id>` to distinguish a menu problem from an alias owned by another plugin.
- Use valid YAML with unique keys and registered action/condition types. A custom label needs `type` or `condition`.

### An item is missing or shows a barrier

Check its material, slot, `showConditions`, nested fallback and sorting group. A missing shared component is unavailable; it does not run the old item's fallback actions. Repair the link or detach a valid resolved copy, then reopen the menu.

### A click does nothing

Check item/action `conditions`, `clickType`, required integrations and the console error. An opening glow effect is not an ordinary click action. For flows, effects belong inside Confirmed actions. See [Click Events](Click-Events.md).

### A placeholder stays as text

Check its spelling and where it is supported. Install PlaceholderAPI and the correct expansion for external tokens. A `%flow_name%` value is available to that flow's confirmed actions, not unrelated later actions. Dynamic text uses `gui-settings.update-delay`.

### Chat input is not accepted

Check the input type, bounds and maximum length, then the configured conditions. Type `exit` to leave a normal prompt; a flow returns to its field screen. Save or cancel existing input before opening another menu. See [Chat Fetcher](Chat-Fetcher.md).

### A reward cannot be claimed

Check the entire bundle fits in the player's inventory, the timer or playtime requirement, and whether that UUID has already claimed it. Menu copies share the same reward progress. If the console reports a pending receipt, inspect saved player inventory before using the recovery commands in [Ready-to-Use Rewards](Ready-to-Use-Rewards.md).

### A warp is missing or the teleport is refused

Check EssentialsX's normal `/warp` command as the same non-OP player, its permissions and the supported name format. The catalog's refresh uses a two-second source cache; clicking a warp rechecks it. GUIPlus does not bypass EssentialsX fees or delays.

### My edit screen expired

Reopen it after a reload, external file change or another accepted revision. An old screen cannot overwrite the newer definition. Save/cancel an in-game draft before editing its file by hand.

### The plugin does not load

Use Java 21 or newer and your server's required version: Paper 26.x needs Java 25 or newer. Check the startup error and repository access for first-start library downloads. V4's server profiles are Paper/Spigot 1.21.11 and Paper 26.3; Folia is unsupported.

### A saved item cannot be read after a downgrade

Return to the matching server/plugin version or restore its matching backup. Do not replace an unreadable storage file with an empty one. See [Updating & Backups](Updating-and-Backups.md).

## 🆘 Getting Help

Include the GUIPlus version, server build, Java version, relevant console message and a small menu example. Mention whether the problem occurs for an operator, an ordinary player or both. Remove private commands or personal data before posting a configuration publicly.
