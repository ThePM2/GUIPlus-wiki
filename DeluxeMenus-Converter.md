> 🔌 **INTEGRATIONS**

# DeluxeMenus Converter

*Migrate supported DeluxeMenus menu definitions to GUIPlus*

---

GUIPlus includes a converter for common DeluxeMenus menu settings, actions and requirements. Conversion creates GUIPlus YAML files for you to review and customize; it does not reproduce every DeluxeMenus feature.

## ⚙ How It Works

1. Keep a copy of your original DeluxeMenus menu files.
2. Place the `.yml` files in `plugins/GUIPlus/DeluxeMenus/`. Use menu definition files containing `menu_title`, `size` and `items`, rather than only DeluxeMenus's menu-registration file.
3. Start the server, or run `/gui reload`.
4. Check the console for `[DM->GUIPlus]` conversion messages and review the generated files in `plugins/GUIPlus/CustomGuis/`.
5. If conversion ran through `/gui reload`, run `/gui reload` again to load the newly created files. During reload, GUIPlus reads existing menu files before it runs the converter.
6. Open a converted menu with `/gui open <id>` and check its layout, requirements and actions before exposing it to players.

The converter reads `.yml` files directly inside the input folder. It accepts a single menu at the file root or menu sections containing the required fields. Menu IDs and output filenames are sanitized to lowercase letters, digits, `_` and `-`.

## 🔄 Re-Converting

Existing output files are skipped rather than overwritten. Editing a DeluxeMenus source file alone does not replace a GUIPlus menu that was already converted.

To convert again, stop the server, move the previous converted output to a backup location outside `CustomGuis/`, update the source file, and start the server. Keep any GUIPlus-only changes separately so you can reapply them.

`/gui reconvert` is for older GUIPlus files in the legacy `inventorys/` folder. It is not the DeluxeMenus conversion command.

The original files in `plugins/GUIPlus/DeluxeMenus/` are not modified by the converter.

## ✅ What Gets Converted

| DeluxeMenus setting | GUIPlus result |
|---|---|
| `menu_title`, `size`, `inventory_type` | Title, row count and inventory type |
| `open_command` | Primary alias and additional aliases from a list |
| Material, amount, display name, lore, item flags | Basic item appearance |
| `slot` or a numeric `slots` list | One generated item entry per slot |
| `basehead-<value>` | Player head with the case-sensitive Base64 value |
| Supported `open_requirement` | Scene `open-conditions` |
| Supported `view_requirement` | Item `showConditions` |
| Supported click requirements | Conditions on the generated actions |
| Left, right, shift-left, shift-right and middle click commands | Actions with the matching `clickType` |
| General `click_commands` | Actions applying to any click type |

Chest size is converted to 1–6 rows. GUIPlus supports chest, hopper, dispenser and dropper layouts; adjust incompatible inventory types manually.

### Supported Requirements

The converter handles `has permission`, `has money` and basic material/amount `has item` requirements, including their `!` inverted forms. Denial commands are limited to `[message]` entries. It rejects unsupported requirement types or options rather than silently dropping an access check.

Click requirements become action conditions. View requirements become `showConditions`, which only control whether an item is displayed. For a critical action, keep an appropriate click requirement too.

### Supported Action Prefixes

The converter recognizes these prefixes:

- `[player]` and `[console]`
- `[message]` and `[close]`
- `[takemoney]` and `[givemoney]`
- `[sound]` and `[broadcastsound]`
- `[openguimenu]` and `[refresh]`
- `[connect]`

`[openguimenu]` links direct item clicks with `open-gui`; an opening-time redirect uses a console `gui open` command. `[refresh]` reopens the current menu when used as a click action and is omitted from opening actions to avoid a loop.

`[connect]` becomes a console `server <name>` command. If that command is unavailable on your backend, replace it with GUIPlus's native `server-click-event` and configure the proxy as described in [BungeeCord Support](BungeeCord-Support.md).

JSON actions, unknown command prefixes and unsupported requirements reject the menu conversion. Read the console message, simplify or replace that part, then convert again. A file containing several menus may have produced earlier menu outputs before a later menu was rejected; inspect the output folder as well as the final message.

## 📝 After Conversion

Review these areas in particular:

- **Aliases and permissions:** Confirm aliases do not conflict with existing commands, and check both menu-opening and button permissions.
- **Same-slot alternatives:** Recreate priority/fallback item arrangements with GUIPlus [Button States](Button-States.md), [nested items](Creating-GUIs.md) or sorting groups. Do not assume DeluxeMenus priority rules were reproduced.
- **Head and item metadata:** `head-` and `hdb-` materials become player heads but do not preserve their owner/database ID automatically. Reconfigure those using [Custom Heads & Skulls](Custom-Heads-and-Skulls.md). Review enchants, custom models and other metadata in the item editor.
- **Commands and sounds:** Confirm command authority, placeholder values, sound identifiers and external-plugin behavior on your server.
- **Other DeluxeMenus options:** Advanced options outside the supported fields may need manual configuration even when the converter produces a file.

Generated actions have an explicit `type`; retain that value when renaming their entry keys. Unknown or unavailable action and condition types prevent a GUI from loading. Correct the configuration or restore the integration that supplies the type, then reload.

Use [Creating GUIs](Creating-GUIs.md), [Click Events](Click-Events.md) and [Conditions](Conditions.md) to finish adapting the menu.

---

| ← Previous | Next → |
|:---|---:|
| [**Plugin Integrations**](Plugin-Integrations.md) | [**Tutorials**](Tutorials.md) |
