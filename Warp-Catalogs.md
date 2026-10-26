> 🔌 **INTEGRATIONS**

# Warp Catalogs

*Build a searchable menu from your server's EssentialsX warps*

---

Warp catalogs generate cards and pages from the actual EssentialsX warp collection. Add or remove a warp in EssentialsX, then reopen or refresh the catalog to update the list. You do not need to author a separate item for every destination.

## 🚀 Open the Catalog

| Command | Permission | Purpose |
|---|---|---|
| `/gui warps` | `guiplus.warps.admin` | Edit catalogs in-game |
| `/guiplus-warps [id]` | `guiplus.warps.use` | Open a player catalog; defaults to `warps` |

The admin permission defaults to operators and also needs the configured `/gui` root permission. The player permission is allowed by default. If another command owns `/guiplus-warps`, use `/guiplus:guiplus-warps`.

Install and enable **EssentialsX 2.x** for the warp provider. The catalog also checks the player's normal `essentials.warp` permission and, when EssentialsX enables per-warp permissions, `essentials.warps.<name>`.

## 🛠 Design a Catalog In-Game

1. Open `/gui warps` and select the default `warps` catalog, or choose **Create warp catalog**.
2. Set **Catalog title** and choose **2–6 rows**.
3. Choose the **content slots** where warp cards may appear. Keep at least one slot; the final row is reserved for navigation.
4. Edit the `card`, `previous`, `next`, `search` and `back` appearances.
5. Use **Preview this draft** to inspect real provider entries for your permissions. Preview cards do not teleport.
6. Choose **Review & Apply**, inspect the complete draft and choose **Apply & save complete draft**.

The editor saves the whole catalog draft. **Discard all changes** or closing discards it. Another accepted edit or an external file change requires reopening a fresh draft after reload.

You can keep **up to 32 catalogs**. IDs use **1–48 lowercase letters, digits, `_` or `-`**, starting with a letter or digit. Titles allow up to **96 characters**; local item names and each local lore line allow up to **256 characters**, with **12 lore lines** per appearance.

## 🔎 Search and Navigation

Players can page through the list, search for part of a warp name and click the page-information book to refresh. Search accepts up to **32 characters**; enter `clear` to show all names. **Back / close** returns from a search to the full list, or closes the full list.

Provider data uses a **two-second cache**. The provider collection is limited to **1,000 entries**; a larger list shows an unavailable state until it is reduced. Missing EssentialsX, an unsupported API, unreadable data and an empty or inaccessible list produce separate messages.

Supported warp names contain **1–64 ASCII letters, digits, `_` or `-`**, starting with a letter, digit or `_`. Names consisting only of digits are omitted because EssentialsX interprets a numeric warp argument as a list page. Names with spaces or extra arguments are also omitted. The catalog shows how many unsupported names were skipped.

## 🏷 Catalog Text and Shared Appearance

Local title, name and lore text can use:

| Token | Value |
|---|---|
| `%catalog_id%` | Catalog ID |
| `%warp_name%` | Warp name on a card |
| `%page%` | Current page, starting at 1 |
| `%pages%` | Total pages |
| `%query%` | Current search text |

For any appearance role, choose **Choose shared component** to use a [Shared Component](Shared-Components.md), or **Use local appearance** to return to the stored local style.

Shared templates use the corresponding component parameters, such as `%component:warp_name%`, `%component:page%` and `%component:query%`. Catalog appearance components must be a single item without state buttons or nested items.

> **Appearance only:** Catalog bindings do not execute a component's actions, conditions or flows. Warp requests, search, page navigation and back behavior remain managed by the catalog. The same component's normal actions still work when linked to an ordinary menu item.

## 📐 Open the Catalog from Your Own Menu

Place this item fragment under `scenes.<scene-id>.items` in an existing three-row GUI. It opens the default catalog as the clicking player:

```yaml
'13':
  slot: 13
  item: COMPASS
  amount: 1
  item-name: '&bBrowse warps'
  item-lore:
    - '&7Search available destinations.'
    - '&eClick to open.'
  click-events:
    '0':
      type: command
      command: 'guiplus:guiplus-warps warps'
      setOp: false
```

Replace the final `warps` argument with your catalog ID to open a different catalog. The player still needs the catalog and EssentialsX permissions described above.

## ✅ Teleport Rules and Saved Data

Clicking a card rechecks its exact warp identity, current provider, command ownership and player access. A removed or renamed warp cannot make an old slot target another destination.

GUIPlus sends the checked `essentials:warp <name>` request as the same player. EssentialsX controls fees, warmup, command cooldowns, world restrictions and the final teleport result. Command preprocessing remains respected; a cancelled or rewritten request is rejected. GUIPlus does not use console, OP or a direct teleport for these cards.

Catalog definitions are saved in `plugins/GUIPlus/WarpCatalogs.yml`. EssentialsX remains the authoritative warp source. Deleting a catalog does not delete EssentialsX warps.

Back up `WarpCatalogs.yml` and `SharedComponents.yml` with the rest of GUIPlus data; automatic GUIPlus backups include both. Keep matching backups when returning to an older plugin version that cannot read these definitions.

---

| ← Previous | Next → |
|:---|---:|
| [**Shared Components**](Shared-Components.md) | [**Plugin Integrations**](Plugin-Integrations.md) |
