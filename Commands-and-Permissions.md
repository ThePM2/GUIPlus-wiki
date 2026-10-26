> ⚙ **GETTING STARTED**

# Commands & Permissions

*Player commands, menu authoring and access settings.*

---

## 📋 Main Commands

The default command is `/gui`, with `/guiplus`, `/gplus` and `/gp` as aliases. Change them under `command-settings.gui-command` in `config.yml`, then restart.

| Command | Description | Permission |
|---------|-------------|------------|
| `/gui` | Open the management menu. | `guiplus.gui` |
| `/gui reload` | Reload settings and menus; open editors and player interactions are retired. | `guiplus.gui.reload` |
| `/gui edit <id>` | Edit a menu in game. | `guiplus.gui.edit` |
| `/gui save` | Save menu definitions. | `guiplus.gui.save` |
| `/gui open <id> [player]` | Open a menu for yourself or a named online player. | `guiplus.gui.open` |
| `/gui openTargeted <id> <target>` | View a menu yourself with its configured actions targeting another online player. | `guiplus.gui.open.targeted` |
| `/gui !!` or `/gui last` | Reopen your last editor page, or the management menu. | `guiplus.gui.openlast` |
| `/gui examples [progression\|unlocks\|choices] [new-id]` | Open the library or install an independent example. | `guiplus.gui.examples` |
| `/gui components` | Manage shared item templates. | `guiplus.components.admin` |
| `/gui warps` | Edit automatic warp catalogs. | `guiplus.warps.admin` |
| `/gui practical edit` | Edit actual reward items and timing. | `guiplus.practical.admin` |
| `/gui practical install` | Add missing practical menus without replacing existing ones. | `guiplus.practical.admin` |
| `/gui practical pending <uuid>` | Inspect an unresolved reward receipt. | `guiplus.practical.admin` |
| `/gui practical resolve <uuid> <receipt> <delivered\|release>` | Reconcile a reward after inspecting the player's saved inventory. | `guiplus.practical.admin` |
| `/gui diagnostics` | Show menu/session counts and runtime measurements. | `guiplus.gui.diagnostics` |
| `/gui reconvert` | Queue old GUIPlus `inventorys/` files for conversion. | `guiplus.gui.reconvert` |

Authoring screens require a player. `/gui open <id> <player>` also works from console. Players opening a menu on somebody else's screen additionally need `guiplus.gui.open.others`.

If you set a root `permission` under `command-settings.gui-command`, it applies before these subcommand permissions. The default `guiplus.gui` entry controls the management screen; it is not a replacement for each subcommand's node.

## 🎁 Player Commands

| Command | Description | Access |
|---------|-------------|--------|
| `/rewards [daily\|milestones\|starter]` | Open the reward hub or a section. | `guiplus.practical.use`, allowed by default. |
| `/guiplus-rewards [daily\|milestones\|starter]` | Reward command when another plugin owns `/rewards`. | Same as above. |
| `/guiplus-warps [id]` | Open an EssentialsX warp catalog. | `guiplus.warps.use`, allowed by default, plus EssentialsX's normal access rules. |
| `/storage [player]` | Open the configured personal storage. | Own storage is available by default; a supplied player name needs `guiplus.storage.others`. |

The default storage aliases are `/vault`, `/playervault` and `/pv`; change or disable them in [Configuration](Configuration.md). When another plugin owns `/guiplus-warps`, use `/guiplus:guiplus-warps`.

The new admin permissions are operator-only by default. The player reward, warp and alias-target permissions declared in the plugin are allowed by default; use your permissions plugin to change that policy.

## 🏷 Menu Aliases and Targets

Each menu can register its own shortcut:

```yaml
id: staff_panel
commandAlias: staffpanel
commandAliasTarget: REQUIRED
permission: myserver.staff
```

`/staffpanel Alex` opens the panel for the sender, with Alex as the action target. It needs the menu permission and `guiplus.gui.alias.target`.

| `commandAliasTarget` | Meaning |
|----------------------|---------|
| `OPTIONAL` | A player name is optional. With a name, the sender views a targeted menu. |
| `REQUIRED` | A player name must be supplied. |
| `DISABLED` | A supplied name identifies the player whose screen opens; players need `guiplus.gui.open.others` for someone else. |

Console always needs a player name and opens the menu for that player. Menu aliases do not require `guiplus.gui.open`, but the menu's permission and scene entry conditions still apply. Names must match an online player exactly.

> **Tip:** Use `/gui open shop Alex` to open a shop on Alex's screen. Use `/gui openTargeted staff_panel Alex` to keep the panel on your own screen while targeting Alex.

## 🔐 Per-Menu Permissions

Use `permission` for the whole menu and `open-conditions` for a particular scene. Item `showConditions` controls visibility; item or action `conditions` controls execution. See [Conditions](Conditions.md).

## 🏦 Bank and Refund Recovery

These administrator commands require `guiplus.transactions`:

```text
/guiplus-transactions list
/guiplus-transactions show <id>
/guiplus-transactions confirm-success <id> <evidence>
/guiplus-transactions confirm-failure <id> <evidence>
/guiplus-transactions refunds
/guiplus-transactions resolve-refund <id> <evidence>
```

Check the economy provider's history first. Confirmation adjusts the local record; it does not repeat an external payout. Use one evidence token, such as `ticket-123`. Reward receipt recovery is separate; see [Ready-to-Use Rewards](Ready-to-Use-Rewards.md).

---

| ← Previous | Next → |
|:---|---:|
| [**Getting Started**](Getting-Started.md) | [**Configuration**](Configuration.md) |
