> ⚙ **GETTING STARTED**

# Updating & Backups

*Update an existing server while keeping menus and player progress.*

---

## 🔄 Update to V4

1. Stop the server normally.
2. Back up the complete `plugins/GUIPlus/` folder and Minecraft player data.
3. Replace the old plugin JAR with one `GUIPlus-4.0.0.jar`. Check the [Java and server requirements](Home.md#requirements).
4. Start the server and read the GUIPlus startup messages.
5. Check your own menus as both an administrator and an ordinary player.
6. Run `/gui practical install` if you want to add the missing reward menus to an existing installation.

The installer keeps existing menu files and reward progress. Ordinary items remain independent until you explicitly link them to a shared component.

## 💾 Automatic Backups

With `disable-automatic-backup: false`, GUIPlus makes a backup before a plugin-version or Minecraft-version migration. If that required backup fails, migration stops so you can fix the disk or access problem first.

Recurring backups default to every 24 hours, checked by the five-minute maintenance task. Change `backup.interval-hours` and `backup.retained-copies` in [Configuration](Configuration.md). Backup ZIPs are kept in `plugins/GUIPlus/backups/`.

They include:

- Menus in `CustomGuis/` and old `inventorys/` definitions.
- `config.yml`, `messages.yml` and `SystemMenus/`.
- Player fields, cooldowns and `playerStorage/`.
- `PracticalRewards.yml`, `SharedComponents.yml` and `WarpCatalogs.yml`.
- Bank transfer and refund records.

Minecraft player inventories are stored by the server separately. Keep a server backup as well, especially when restoring rewards or storage.

## 📝 Editing Existing Files

Use one editing path at a time. Save or cancel an in-game draft before changing its YAML, then reload. A reload closes old sessions; reopen the editor or player menu afterwards.

A malformed definition or missing required integration can make the affected menu unavailable. Read the console error and fix its source file. Do not remove a requirement simply to make the menu load.

Custom `SystemMenus/` layouts are retained. An older custom layout may not contain new controls, so use `/gui examples`, `/gui practical edit`, `/gui components` and `/gui warps` directly. In older ordinary item editors, Shift-right-click **Nested Items** opens Button States; Shift-left-click opens Shared Component.

## ↩ Rolling Back

Restore the matching data backup together with the older JAR. V4 state buttons, flows, components and warp catalogs need a plugin version that understands those definitions. Replacing only the JAR is not a data rollback.

The same applies when downgrading Minecraft: newer item data may be unreadable on an older server. Keep the original backup until you have checked your menus and stored items.

## 🏦 Pending Operations

An interrupted external economy call or reward delivery may need manual reconciliation. Check the provider history or saved player inventory before making a decision.

- Rewards: [Ready-to-Use Rewards](Ready-to-Use-Rewards.md).
- Bank transfers and refunds: [Commands & Permissions](Commands-and-Permissions.md#bank-and-refund-recovery).

These decisions update saved records; they do not automatically repeat an uncertain payout or grant items again.
