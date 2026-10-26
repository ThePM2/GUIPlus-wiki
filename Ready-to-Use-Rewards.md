> 📖 **TUTORIALS & RESOURCES**

# Ready-to-Use Rewards

*Give players daily prizes, playtime milestones and one starter choice*

---

GUIPlus includes three practical reward menus that grant real vanilla items. They work without Vault, PlaceholderAPI, a permissions plugin or a resource pack.

## 🚀 Open the Menus

On a fresh installation, start the server and use:

| Command | Opens |
|---|---|
| `/rewards` | Reward hub |
| `/rewards daily` | Daily Rewards |
| `/rewards milestones` | Playtime Milestones |
| `/rewards starter` | Starter Choice |

`/guiplus-rewards` accepts the same page arguments. If another plugin or an authored GUI already owns `/rewards`, its command is preserved; use `/guiplus-rewards`.

For an existing installation, run `/gui practical install` once as an administrator. It installs only missing menus and preserves existing files, live menus and progress. Deleting a reward menu does not reinstall it on reload; use the install command to restore a missing menu.

| Permission | Default | Purpose |
|---|---|---|
| `guiplus.practical.use` | Everyone | Open and use the player menus |
| `guiplus.practical.admin` | Operators | Install menus, edit actual rewards and reconcile pending receipts |

Administrator subcommands also require the root permission configured for `/gui`. Player reward access does not grant GUI editing or other-player targeting.

## 📅 Daily Rewards

The first prize is available immediately. After a successful claim, wait **24 hours** by default before taking the next prize. The administrator can change that interval in the reward editor.

| Step / reward ID | Default prize |
|---|---|
| `daily-1` | 8 bread |
| `daily-2` | 16 torches |
| `daily-3` | 32 oak planks |
| `daily-4` | 8 cooked beef |
| `daily-5` | 8 coal |
| `daily-6` | 4 experience bottles |
| `daily-7` | 4 iron ingots |

Missing a day keeps progress. After reward seven and its wait, the next seven-prize cycle begins. This is a rolling claim timer: it has no midnight reset or consecutive-login requirement. Viewing, reconnecting and restarting do not start or reset the timer. Players claim through the menu; joining does not grant a reward automatically.

## ⏱ Playtime Milestones

Each milestone can be claimed once per player UUID. The requirements use Minecraft's existing total playtime statistic, including AFK time. Players may already qualify when the menus are installed.

| Reward ID | Default total playtime | Default prize |
|---|---|---|
| `playtime-30m` | 30 minutes | 16 bread |
| `playtime-2h` | 2 hours | 8 iron ingots |
| `playtime-6h` | 6 hours | 4 experience bottles |
| `playtime-12h` | 12 hours | 1 diamond |

Resetting server playtime statistics does not erase completed GUIPlus claims. Changing a threshold affects unclaimed milestones; it does not make a completed milestone claimable again.

## 🎒 Starter Choice

Choose a kit, review it and **Confirm**. Back lets the player change their answer. Cancel grants nothing and keeps the choice available.

| Reward ID | Default bundle |
|---|---|
| `starter-builder` | 64 oak planks, 64 cobblestone, 16 bread |
| `starter-explorer` | Stone pickaxe, 32 torches, 16 bread, compass |
| `starter-farmer` | Stone hoe, 32 wheat seeds, 16 bread, water bucket |

There is **one starter choice per player UUID**, shared by all three options. Copying, renaming or reopening a menu does not grant another starter kit. Players have no progress-reset control.

## 🛠 Edit the Actual Prizes

Open `/gui practical edit`, or choose **Edit actual rewards** in the Example Library.

1. Browse **Daily**, **Playtime** or **Starter**.
2. Edit a reward's actual item stacks: material, amount, name and lore, or copy an item held in your main hand with its metadata.
3. Left-click a held-copy control to copy immediately. Right-click it to close the inventory temporarily, select another hotbar item, then type `copy`. Type `exit` to return without copying.
4. Set the daily interval or the selected playtime requirement.
5. Choose **Apply & save all rewards** to save the complete catalog.

The source item is never consumed. **Cancel all changes** or closing discards the complete draft across all pages.

Each reward needs **1–27 item stacks**. Amounts must fit the item's normal stack limit and the **64** cap; use another entry for extra tools or stacks. Time settings accept **1–525,600 whole minutes**. Empty, unreadable or incompatible items are rejected.

The catalog is saved in `plugins/GUIPlus/PracticalRewards.yml`. Another accepted edit or an external file change makes an older draft stale. Reopen after reloading an externally changed file.

**Changes affect future claims.** An active daily wait keeps its saved deadline; new daily claims use the new interval. The 14 reward IDs above remain fixed, so editing prizes does not reset claims. Players must reopen a review that was opened before a catalog change.

## 🎨 Edit the Menus and Display Current Prizes

The layouts are ordinary menus: `gp_rewards`, `gp_daily`, `gp_milestones` and `gp_starter`. Use the normal editor for appearance, text and navigation. Edit the actual prize in the reward editor described above.

These local tokens work without PlaceholderAPI:

| Token | Displays |
|---|---|
| `%GUIPlus_reward_daily_total%` | Lifetime successful daily claims |
| `%GUIPlus_reward_daily_day%` | Next daily step |
| `%GUIPlus_reward_daily_remaining%` | Formatted time until the next daily claim |
| `%GUIPlus_reward_daily_interval%` | Current configured daily interval |
| `%GUIPlus_reward_playtime%` | Total playtime in minutes, with a `min` suffix |
| `%GUIPlus_reward_starter_choice%` | Saved starter choice, or `none` |
| `%GUIPlus_reward_threshold_playtime-30m%` | Current requirement for that stable milestone ID |
| `%GUIPlus_reward_bundle_daily-1%` | Actual configured items for that reward ID |

Use any canonical reward ID with `bundle_`, and any playtime reward ID with `threshold_`. Put a bundle token **alone on its lore line** to expand it into separate lines. Player descriptions show up to eight stacks and a count of any additional stacks; the admin editor shows the whole bundle.

For example, add this information item under `scenes.<scene-id>.items` in an existing three-row menu:

```yaml
'11':
  slot: 11
  item: CLOCK
  amount: 1
  item-name: '&eYour daily rewards'
  item-lore:
    - '&7Next claim: %GUIPlus_reward_daily_remaining%'
    - '&7Claim interval: %GUIPlus_reward_daily_interval%'
    - '&7Total claimed: %GUIPlus_reward_daily_total%'
    - '&7First reward contains:'
    - '%GUIPlus_reward_bundle_daily-1%'
```

For your own claim buttons, use **Claim practical reward** (`type: practical-reward`, `reward: 'daily-1'`) and **Practical reward status** (`condition: practical-reward-status`). Status values are `LOCKED`, `READY`, `OWNED` and `BLOCKED`. See the complete [Button States example](Button-States.md) for a button that displays the claim state and grants the configured reward.

## 💾 Saved Progress and Recovery

Claims are stored by UUID in `playerData.yml`, under `guiplus_practical_v1`. Back up that file, `PracticalRewards.yml` and normal Minecraft player data together. Installing or restoring menu files leaves claims intact; do not delete player data to repair a missing menu.

A full inventory grants nothing and consumes no claim or daily timer. The whole bundle must fit in storage slots and partial stacks. GUIPlus does not drop overflow rewards on the ground.

Before delivery, GUIPlus saves a reservation, then applies and saves the inventory before completing the claim. Plugin progress and Minecraft inventory are separate files. A crash between those steps can leave a pending receipt that blocks further claims for staff review.

| Administrator command | Effect |
|---|---|
| `/gui practical pending <uuid>` | Show the pending reward and receipt |
| `/gui practical resolve <uuid> <receipt> delivered` | Record its proposed progress after delivery has been checked |
| `/gui practical resolve <uuid> <receipt> release` | Remove the reservation and allow another attempt |

Neither resolution command grants, removes or replays items. **Check the saved inventory before resolving a receipt.** Releasing one that was delivered can allow a duplicate claim.

A malformed catalog at startup disables practical rewards while preserving progress and ordinary menus. A failed reload keeps the last accepted catalog active and leaves the file untouched. Repair or restore `PracticalRewards.yml`, then reload. Existing pending receipts can still be reconciled independently of the current catalog.

---

| ← Previous | Next → |
|:---|---:|
| [**Input Rules**](Input-Rules.md) | [**Shared Components**](Shared-Components.md) |
