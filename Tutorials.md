> 📖 **TUTORIALS**

# Tutorials

*Step-by-step guides for building common server GUIs*

---

These tutorials walk you through building practical, real-world GUIs from scratch. Each tutorial builds on GUIPlus features and shows how they work together. The snippets within each tutorial belong in the same file; keep the shown indentation when combining them.

Use an action ID such as `message` directly, or give a named entry an explicit `type: message`. Unknown action and condition IDs prevent the affected GUI from loading. Item `conditions` guard clicks; `showConditions` control visibility. See [Click Events](Click-Events.md) and [Conditions](Conditions.md) for the full reference.

## 🗺 Tutorial 1: Warp Menu

A categorized warp menu where players can teleport to different locations. This example runs commands supplied by your warp plugin, such as EssentialsX. Create those destinations first and give players the matching command permissions. For a menu that automatically follows EssentialsX warps, see [Warp Catalogs](Warp-Catalogs.md).

### Step 1: Create the GUI File

Create `plugins/GUIPlus/CustomGuis/warps.yml`:

```yaml
id: warps
rows: 3
type: chest
title: '§5§lServer Warps'
commandAlias: warps
```

### Step 2: Add Warp Items

Each warp item uses a `command` click event to run the warp command as the player, and a `sound-click-event` for feedback:

```yaml
scenes:
  '0':
    delay: 0
    items:
      '1':
        slot: 10
        item: GRASS_BLOCK
        amount: 1
        item-name: '§a§lSpawn'
        item-lore:
          - ''
          - '§7The main server spawn'
          - '§7Safe zone with shops nearby'
          - ''
          - '§eClick to teleport!'
        click-events:
          sound-click-event:
            sound: ENTITY_ENDERMAN_TELEPORT
          command:
            commands:
              - spawn
          close-inventory: {}
      '2':
        slot: 12
        item: DIAMOND_ORE
        amount: 1
        item-name: '§b§lMines'
        item-lore:
          - ''
          - '§7Public mining area'
          - '§7Ores reset every 30 minutes'
          - ''
          - '§eClick to teleport!'
        click-events:
          sound-click-event:
            sound: ENTITY_ENDERMAN_TELEPORT
          command:
            commands:
              - warp mines
          close-inventory: {}
      '3':
        slot: 14
        item: GOLDEN_SWORD
        amount: 1
        item-name: '§c§lPvP Arena'
        item-lore:
          - ''
          - '§7Fight other players'
          - '§7Keep your inventory on death'
          - ''
          - '§eClick to teleport!'
        click-events:
          sound-click-event:
            sound: ENTITY_ENDERMAN_TELEPORT
          command:
            commands:
              - warp pvp
          close-inventory: {}
      '4':
        slot: 16
        item: OAK_SAPLING
        amount: 1
        item-name: '§2§lFarm'
        item-lore:
          - ''
          - '§7Community farms and plots'
          - ''
          - '§eClick to teleport!'
        conditions:
          has-permission:
            permission: server.farmer
        conditionFailMessage: '§cYou need the Farmer rank!'
        click-events:
          sound-click-event:
            sound: ENTITY_ENDERMAN_TELEPORT
          command:
            commands:
              - warp farm
          close-inventory: {}
```

### Step 3: Add a VIP-Only Warp

Use `showConditions` to hide a VIP warp and repeat the permission under `conditions` to guard its click:

```yaml
      '5':
        slot: 22
        item: NETHER_STAR
        amount: 1
        item-name: '§6§lVIP Lounge'
        item-lore:
          - ''
          - '§7Exclusive VIP area'
          - '§7Special vendors and rewards'
          - ''
          - '§6Click to teleport!'
        showConditions:
          has-permission:
            permission: server.vip
        conditions:
          has-permission:
            permission: server.vip
        conditionFailMessage: '&cThis warp is for VIP players.'
        click-events:
          sound-click-event:
            sound: ENTITY_PLAYER_LEVELUP
          command:
            commands:
              - warp vip
          close-inventory: {}
```

### Result

Players type `/warps` to open a menu with teleport options. The Farm button remains visible and rejects clicks without `server.farmer`. The VIP Lounge is hidden without `server.vip`. The warp plugin also applies its own teleport rules.

---

## 🛒 Tutorial 2: In-Game Shop and Amount Input

A small shop with a fixed-price item and a bounded donation prompt. Install Vault and a working Vault economy provider before using the money actions.

### Step 1: Create the Shop

Create `plugins/GUIPlus/CustomGuis/donationshop.yml`:

```yaml
id: donationshop
rows: 3
type: chest
title: '&6&lIn-Game Shop'
commandAlias: donate
scenes:
  '0':
    delay: 0
    items:
      '1':
        slot: 4
        item: GOLD_INGOT
        amount: 1
        item-name: '&e&lShop Information'
        item-lore:
          - '&7Pay with your in-game balance.'
          - '&7Sword: &a$500'
```

### Step 2: Add a Fixed-Price Item

Add this item under the same `items` section. Left-click buys one sword through GUIPlus's native item-give action. Right-click displays information. Put purchase checks on the first left-click action so they do not block the information click.

```yaml
      '2':
        slot: 11
        item: DIAMOND_SWORD
        amount: 1
        item-name: '&b&lDiamond Sword'
        item-lore:
          - '&7Price: &a$500'
          - '&eLeft-click to buy.'
          - '&7Right-click for information.'
        click-events:
          purchase-cost:
            type: money-remove
            amount: 500
            clickType: LEFT
            conditions:
              has-money:
                required-balance: 500
              has-empty-slots:
                free-slots: 1
            conditionFailMessage: '&cYou need $500 and one empty inventory slot.'
          purchase-item:
            type: custom-item-give
            clickType: LEFT
            item:
              type: DIAMOND_SWORD
              amount: 1
          purchase-message:
            type: message
            clickType: LEFT
            message: '&aPurchased a Diamond Sword for $500!'
          purchase-sound:
            type: sound-click-event
            clickType: LEFT
            sound: ENTITY_PLAYER_LEVELUP
          information:
            type: message
            clickType: RIGHT
            message: '&7One ordinary Diamond Sword costs $500.'
```

The displayed menu item and the delivered reward are separate. To deliver a named or enchanted sword, configure the `custom-item-give` reward through the in-game editor so it stores the complete item.

### Step 3: Add a Bounded Amount Prompt

Add this donation button under `items`. It accepts a whole-number amount from 1 to 1000. The nested actions run only after the input passes validation; `exit` cancels the prompt.

```yaml
      '3':
        slot: 15
        item: GOLDEN_APPLE
        amount: 1
        item-name: '&6&lDonate In-Game Money'
        item-lore:
          - '&7Choose an amount from $1 to $1000.'
        click-events:
          chat-fetcher:
            message: '&eHow much would you like to donate? &7(Type exit to cancel.)'
            disable-exit-message: false
            input-type: INTEGER
            input-min: 1
            input-max: 1000
            input-max-length: 4
            conditionFailMessage: '&cYou do not have enough money.'
            conditions:
              has-money:
                required-balance: '%input%'
            click-events:
              money-remove:
                amount: '%input%'
              message:
                message: '&aThank you for donating $%input%!'
              sound-click-event:
                sound: ENTITY_PLAYER_LEVELUP
```

`amount` expects a number after placeholders are resolved. Text such as `'%input%*50'` is not a built-in calculation. See [Input Rules](Input-Rules.md) for other input types and bounds.

### Step 4: Add a Close Button

```yaml
      '4':
        slot: 22
        item: BARRIER
        amount: 1
        item-name: '&c&lClose'
        click-events:
          close-inventory: {}
```

Run `/gui reload`, then open `/donate`. To add a quantity picker followed by an explicit confirmation screen, use [Interaction Flows](Interaction-Flows.md). External shop commands may have their own refusal or delivery rules; an action list cannot make arbitrary plugins share one transaction.

---

## 📊 Tutorial 3: Player Statistics Dashboard

A dynamic profile menu that displays live player data using PlaceholderAPI. This tutorial shows advanced placeholder usage and dynamic content.

### Step 1: Set Up Dependencies

Make sure you have these plugins installed:

- **PlaceholderAPI** — Core placeholder support
- **Vault** and an economy provider — Balance data
- The Player, Statistic, Vault and LuckPerms placeholder providers used below; install their required expansions and check each placeholder with `/papi parse me <placeholder>`

### Step 2: Create the Dashboard

```yaml
id: stats
rows: 5
type: chest
title: '§3§l%player_name%''s Dashboard'
commandAlias: stats
scenes:
  '0':
    delay: 0
    items:
      # Player head with overview
      '1':
        slot: 4
        item: PLAYER_HEAD
        amount: 1
        item-name: '§e§l%player_name%'
        item-lore:
          - ''
          - '§7Rank: §f%luckperms_primary_group%'
          - '§7Balance: §a$%vault_eco_balance_formatted%'
          - '§7First Joined: §b%player_first_join_date%'
          - ''
        skullBase64: '%GUIPlus_self_base64%'

      # Combat stats
      '2':
        slot: 19
        item: DIAMOND_SWORD
        amount: 1
        item-name: '§c§lCombat Stats'
        item-lore:
          - ''
          - '§7Kills: §f%statistic_player_kills%'
          - '§7Deaths: §f%statistic_deaths%'
          - '§7Damage Dealt: §f%statistic_damage_dealt%'
          - ''

      # Mining stats
      '3':
        slot: 21
        item: DIAMOND_PICKAXE
        amount: 1
        item-name: '§b§lMining Stats'
        item-lore:
          - ''
          - '§7Diamonds Found: §f%statistic_mine_block_diamond_ore%'
          - '§7Iron Mined: §f%statistic_mine_block_iron_ore%'
          - '§7Gold Mined: §f%statistic_mine_block_gold_ore%'
          - ''

      # Playtime
      '4':
        slot: 23
        item: CLOCK
        amount: 1
        item-name: '§a§lPlay Time'
        item-lore:
          - ''
          - '§7Total: §f%statistic_time_played%'
          - '§7Since Last Death: §f%statistic_time_since_death%'
          - ''

      # Economy info
      '5':
        slot: 25
        item: GOLD_INGOT
        amount: 1
        item-name: '§6§lEconomy'
        item-lore:
          - ''
          - '§7Balance: §a$%vault_eco_balance_formatted%'
          - '§7Bank: §e$%GUIPlus_player_info_deposited%'
          - '§7Points: §d%GUIPlus_player_info_points%'
          - ''

      # Navigation: close
      '6':
        slot: 40
        item: BARRIER
        amount: 1
        item-name: '§c§lClose'
        click-events:
          close-inventory: {}
```

### How It Works

Dynamic item text refreshes while the GUI is open. The default `gui-settings.update-delay` is 20 ticks, about one second at 20 TPS. Each external placeholder needs its own installed provider. `deposited` and `points` in this example are GUIPlus player-data fields; initialize or populate them for your server rather than assuming they are external bank or PlayerPoints balances.

---

## 🎁 Tutorial 4: Multi-Page Crate Rewards Preview

A paginated rewards preview for server crates. This demonstrates scenes with manual navigation. The reward names and chances are display text: this menu does not read a crate plugin or award items. Update the preview when you change your crate configuration.

### Step 1: Create the Structure

```yaml
id: craterewards
disable-scene-animation: true
rows: 6
type: chest
title: '§d§lCrate Rewards Preview'
commandAlias: rewards
```

### Step 2: Build Page 1

```yaml
scenes:
  '0':
    delay: 0
    open-events:
      page-sound:
        type: sound-click-event
        sound: BLOCK_CHEST_OPEN
    items:
      # Header
      '1':
        slot: 4
        item: CHEST
        amount: 1
        item-name: '§d§lVote Crate'
        item-lore:
          - ''
          - '§7Preview of all possible rewards'
          - '§7Page §e1§7/§e2'
          - ''

      # Reward items
      '2':
        slot: 19
        item: DIAMOND
        amount: 5
        item-name: '§bDiamonds x5'
        item-lore:
          - '§7Chance: §a15%'
      '3':
        slot: 20
        item: EMERALD
        amount: 3
        item-name: '§aEmeralds x3'
        item-lore:
          - '§7Chance: §a20%'
      '4':
        slot: 21
        item: IRON_INGOT
        amount: 16
        item-name: '§fIron Ingots x16'
        item-lore:
          - '§7Chance: §a25%'
      '5':
        slot: 22
        item: GOLDEN_APPLE
        amount: 2
        item-name: '§6Golden Apples x2'
        item-lore:
          - '§7Chance: §e10%'
      '6':
        slot: 23
        item: EXPERIENCE_BOTTLE
        amount: 10
        item-name: '§aXP Bottles x10'
        item-lore:
          - '§7Chance: §a30%'

      # Navigation
      '50':
        slot: 53
        item: ARROW
        amount: 1
        item-name: '§eNext Page →'
        item-lore:
          - '§7Click for more rewards'
        click-events:
          next-scene-click: {}
          sound-click-event:
            sound: UI_BUTTON_CLICK

      # Close
      '51':
        slot: 49
        item: BARRIER
        amount: 1
        item-name: '§cClose'
        click-events:
          close-inventory: {}
```

### Step 3: Build Page 2

```yaml
  '1':
    delay: 0
    open-events:
      page-sound:
        type: sound-click-event
        sound: UI_BUTTON_CLICK
    items:
      # Header
      '1':
        slot: 4
        item: CHEST
        amount: 1
        item-name: '§d§lVote Crate'
        item-lore:
          - ''
          - '§7Preview of all possible rewards'
          - '§7Page §e2§7/§e2'
          - ''

      # Rare rewards
      '2':
        slot: 19
        item: NETHERITE_INGOT
        amount: 1
        item-name: '§4§lNetherite Ingot'
        item-lore:
          - '§7Chance: §c1%'
          - '§6§lLEGENDARY'
      '3':
        slot: 20
        item: ELYTRA
        amount: 1
        item-name: '§d§lElytra'
        item-lore:
          - '§7Chance: §c0.5%'
          - '§6§lLEGENDARY'
      '4':
        slot: 21
        item: TOTEM_OF_UNDYING
        amount: 1
        item-name: '§e§lTotem of Undying'
        item-lore:
          - '§7Chance: §c2%'
          - '§5§lEPIC'

      # Navigation
      '50':
        slot: 45
        item: ARROW
        amount: 1
        item-name: '§e← Previous Page'
        item-lore:
          - '§7Go back to page 1'
        click-events:
          previous-scene-click: {}
          sound-click-event:
            sound: UI_BUTTON_CLICK

      # Close
      '51':
        slot: 49
        item: BARRIER
        amount: 1
        item-name: '§cClose'
        click-events:
          close-inventory: {}
```

---

## 🛡 Tutorial 5: Staff Moderation Panel

A staff menu that sends fixed moderation commands after a player is selected. The permission on the GUI controls who can open it; console actions run with console authority. Use the moderation plugin's own logs for an audit trail. GUIPlus does not create a punishment history from these buttons.

### Step 1: The Main Menu

```yaml
id: punish
rows: 3
type: chest
title: '§4§lStaff Panel'
commandAlias: punish
permission: staff.punish
scenes:
  '0':
    delay: 0
    items:
      '1':
        slot: 10
        item: DAMAGED_ANVIL
        amount: 1
        item-name: '§c§lBan Player'
        item-lore:
          - ''
          - '§7Permanently ban a player'
          - '§7from the server'
          - ''
          - '§cClick to select player'
        click-events:
          player-picker-by-console-command:
            command: 'ban %player% Banned by %executor%'
          message:
            message: '§7Ban command sent for %player% by %executor%.'
          sound-click-event:
            sound: BLOCK_ANVIL_LAND

      '2':
        slot: 12
        item: IRON_DOOR
        amount: 1
        item-name: '§e§lKick Player'
        item-lore:
          - ''
          - '§7Kick a player from'
          - '§7the server'
          - ''
          - '§eClick to select player'
        click-events:
          player-picker-by-console-command:
            command: 'kick %player% Kicked by %executor%'
          message:
            message: '§7Kick command sent for %player% by %executor%.'

      '3':
        slot: 14
        item: WRITABLE_BOOK
        amount: 1
        item-name: '§b§lWarn Player'
        item-lore:
          - ''
          - '§7Send a warning with'
          - '§7a fixed staff reason'
          - ''
          - '§bClick to select player'
        click-events:
          player-picker-by-console-command:
            command: 'warn %player% Warned by %executor%'
          message:
            message: '§7Warning command sent for %player% by %executor%.'

      '4':
        slot: 16
        item: LECTERN
        amount: 1
        item-name: '§a§lPardon Player'
        item-lore:
          - ''
          - '§7Unban a player'
          - '§7(includes offline players)'
          - ''
          - '§aClick to select player'
        click-events:
          offline-player-picker-command:
            command: 'unban %player%'
          message:
            message: '§7Pardon command sent for %player% by %executor%.'
```

### Step 2: Enter a Player Name

For a typed name rather than a picker, use the `PLAYER_NAME` input rule. This validates the name format; it does not prove that the account exists or is online. The example sends a fixed reason. Replace `warn` or `unban` above with commands supplied by your moderation plugin (`minecraft:pardon` is the vanilla pardon command).

```yaml
      '5':
        slot: 22
        item: WITHER_SKELETON_SKULL
        amount: 1
        item-name: '§4§lBan by Name'
        item-lore:
          - ''
          - '§7Enter a player name and'
          - '§7use a fixed staff reason'
          - ''
          - '§4Click to start'
        click-events:
          chat-fetcher:
            message: '§eType the player name to ban:'
            disable-exit-message: false
            input-type: PLAYER_NAME
            input-max-length: 16
            click-events:
              console_command:
                commands:
                  - 'ban %input% Custom ban by %player%'
              message:
                message: '§7Ban command sent for %input%.'
```

---

## 🚀 What's Next?

- 🎁 Install [Ready-to-Use Rewards](Ready-to-Use-Rewards.md) for daily rewards, playtime milestones and a starter choice
- 🧩 Reuse common controls with [Shared Components](Shared-Components.md)
- 🗺 Build an automatic EssentialsX menu with [Warp Catalogs](Warp-Catalogs.md)
- 🔄 Show ready, locked and cooldown appearances with [Button States](Button-States.md)
- 📝 Add selection and confirmation with [Interaction Flows](Interaction-Flows.md) and [Input Rules](Input-Rules.md)
- 📦 Browse [Premade Configurations](Premade-Configurations.md) for GUI templates
- 🧩 Learn about [Plugin Integrations](Plugin-Integrations.md) for connecting with other plugins
- 💡 Read [Tips & Best Practices](Tips-and-Best-Practices.md) for optimization advice

---

| ← Previous | Next → |
|:---|---:|
| [**DeluxeMenus Converter**](DeluxeMenus-Converter.md) | [**Premade Configurations**](Premade-Configurations.md) |
