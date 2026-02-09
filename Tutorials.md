> 📖 **TUTORIALS**

# Tutorials

*Step-by-step guides for building common server GUIs*

---

These tutorials walk you through building practical, real-world GUIs from scratch. Each tutorial builds on GUIPlus features and shows how they work together.

## 🗺 Tutorial 1: Warp Menu

A categorized warp menu where players can teleport to different locations. This demonstrates basic click events, conditions, and item layout.

### Step 1: Create the GUI File

Create `plugins/GUIPlus/CustomGuis/warps.yml`:

```yaml
id: warps
rows: 3
type: chest
title: §5§lServer Warps
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
        item-name: §a§lSpawn
        item-lore:
          - ''
          - §7The main server spawn
          - §7Safe zone with shops nearby
          - ''
          - §eClick to teleport!
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
        item-name: §b§lMines
        item-lore:
          - ''
          - §7Public mining area
          - §7Ores reset every 30 minutes
          - ''
          - §eClick to teleport!
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
        item-name: §c§lPvP Arena
        item-lore:
          - ''
          - §7Fight other players
          - §7Keep your inventory on death
          - ''
          - §eClick to teleport!
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
        item-name: §2§lFarm
        item-lore:
          - ''
          - §7Community farms and plots
          - ''
          - §eClick to teleport!
        conditions:
          has-permission:
            permission: server.farmer
        conditionFailMessage: §cYou need the Farmer rank!
        click-events:
          sound-click-event:
            sound: ENTITY_ENDERMAN_TELEPORT
          command:
            commands:
              - warp farm
          close-inventory: {}
```

### Step 3: Add a VIP-Only Warp with Fallback

Use a condition to show a VIP warp only to VIP players. Non-VIP players see a locked version using a sorting group (set via the in-game editor):

```yaml
      '5':
        slot: 22
        item: NETHER_STAR
        amount: 1
        item-name: §6§lVIP Lounge
        item-lore:
          - ''
          - §7Exclusive VIP area
          - §7Special vendors and rewards
          - ''
          - §6Click to teleport!
        conditions:
          has-permission:
            permission: server.vip
        click-events:
          sound-click-event:
            sound: ENTITY_PLAYER_LEVELUP
          command:
            commands:
              - warp vip
          close-inventory: {}
```

### Result

Players type `/warps` to open a menu with teleport options. Protected warps are hidden from players who lack the required permissions.

---

## 🛒 Tutorial 2: Donation Shop with Confirmation

A shop where players spend in-game money. This tutorial shows economy integration, chat fetcher for quantity input, and multi-step confirmation.

### Step 1: Create the Shop

```yaml
id: donationshop
rows: 4
type: chest
title: §6§lDonation Shop
commandAlias: donate
scenes:
  '0':
    delay: 0
    items:
      '1':
        slot: 4
        item: PLAYER_HEAD
        amount: 1
        item-name: §e§l%player_name%'s Shop
        item-lore:
          - ''
          - §7Your Balance: §a$%vault_eco_balance_formatted%
          - ''
        skullBase64: '%self_base64%'
```

### Step 2: Add Shop Items with Economy Checks

Each item checks the player's balance before allowing purchase:

```yaml
      '2':
        slot: 19
        item: DIAMOND_SWORD
        amount: 1
        item-name: §c§lDiamond Sword
        item-lore:
          - ''
          - §7A powerful weapon
          - ''
          - §7Price: §a$500
          - §7Your Balance: §a$%vault_eco_balance_formatted%
          - ''
          - §eLeft-click to buy!
          - §7Right-click for info
        item-enchants:
          DAMAGE_ALL: 3
        item-flags:
          - HIDE_ENCHANTS
        conditions:
          has-money:
            required-balance: 500
        conditionFailMessage: §cYou need at least $500!
        click-events:
          buy-action:
            message: §aPurchased Diamond Sword for $500!
            clickType: LEFT
          buy-sound:
            sound: ENTITY_PLAYER_LEVELUP
            clickType: LEFT
          buy-remove:
            amount: 500
            clickType: LEFT
          buy-give:
            commands:
              - give %player% diamond_sword{Enchantments:[{id:sharpness,lvl:3}]} 1
            clickType: LEFT
          info-message:
            message: §7Diamond Sword — Sharpness III. Costs $500.
            clickType: RIGHT
```

### Step 3: Add a Custom Amount Purchase

Let players choose how many of an item to buy using chat fetcher:

```yaml
      '3':
        slot: 21
        item: GOLDEN_APPLE
        amount: 1
        item-name: §6§lGolden Apples
        item-lore:
          - ''
          - §7Price: §a$50 each
          - §7Your Balance: §a$%vault_eco_balance_formatted%
          - ''
          - §eClick to choose amount!
        click-events:
          chat-fetcher:
            message: §eHow many Golden Apples would you like to buy? §7(Type a number)
            conditionFailMessage: §cInvalid number or not enough money!
            conditions:
              is-integer:
                value: '%input%'
              conditional-placeholder:
                conditional_condition: '%input%>0'
            click-events:
              money-remove:
                amount: '%input%*50'
              console_command:
                commands:
                  - give %player% golden_apple %input%
              message:
                message: §aPurchased %input% Golden Apples!
              sound-click-event:
                sound: ENTITY_PLAYER_LEVELUP
```

### Step 4: Add Navigation Buttons

```yaml
      # Glass pane border (decorative)
      '10':
        slot: 27
        item: BLACK_STAINED_GLASS_PANE
        amount: 1
        item-name: ' '
      '11':
        slot: 35
        item: BARRIER
        amount: 1
        item-name: §c§lClose
        item-lore:
          - §7Click to close the shop
        click-events:
          close-inventory: {}
```

---

## 📊 Tutorial 3: Player Statistics Dashboard

A dynamic profile menu that displays live player data using PlaceholderAPI. This tutorial shows advanced placeholder usage and dynamic content.

### Step 1: Set Up Dependencies

Make sure you have these plugins installed:
- **PlaceholderAPI** — Core placeholder support
- **Vault** — Economy data
- Any PAPI expansion you want (run `/papi ecloud download Player`, `/papi ecloud download Statistic`, etc.)

### Step 2: Create the Dashboard

```yaml
id: stats
rows: 5
type: chest
title: §3§l%player_name%'s Dashboard
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
        item-name: §e§l%player_name%
        item-lore:
          - ''
          - §7Rank: §f%luckperms_primary_group%
          - §7Balance: §a$%vault_eco_balance_formatted%
          - §7First Joined: §b%player_first_join_date%
          - ''
        skullBase64: '%self_base64%'

      # Combat stats
      '2':
        slot: 19
        item: DIAMOND_SWORD
        amount: 1
        item-name: §c§lCombat Stats
        item-lore:
          - ''
          - §7Kills: §f%statistic_player_kills%
          - §7Deaths: §f%statistic_deaths%
          - §7K/D Ratio: §f%math_{statistic_player_kills}/{statistic_deaths}%
          - §7Damage Dealt: §f%statistic_damage_dealt%
          - ''

      # Mining stats
      '3':
        slot: 21
        item: DIAMOND_PICKAXE
        amount: 1
        item-name: §b§lMining Stats
        item-lore:
          - ''
          - §7Blocks Mined: §f%statistic_mine_block%
          - §7Diamonds Found: §f%statistic_mine_block_diamond_ore%
          - §7Iron Mined: §f%statistic_mine_block_iron_ore%
          - §7Gold Mined: §f%statistic_mine_block_gold_ore%
          - ''

      # Playtime
      '4':
        slot: 23
        item: CLOCK
        amount: 1
        item-name: §a§lPlay Time
        item-lore:
          - ''
          - §7Total: §f%statistic_time_played%
          - §7Since Last Death: §f%statistic_time_since_death%
          - ''

      # Economy info
      '5':
        slot: 25
        item: GOLD_INGOT
        amount: 1
        item-name: §6§lEconomy
        item-lore:
          - ''
          - §7Balance: §a$%vault_eco_balance_formatted%
          - §7Bank: §e$%GUIPlus_player_info_deposited%
          - §7Points: §d%GUIPlus_player_info_points%
          - ''

      # Navigation: close
      '6':
        slot: 40
        item: BARRIER
        amount: 1
        item-name: §c§lClose
        click-events:
          close-inventory: {}
```

### How It Works

Every placeholder in the lore is updated automatically while the GUI is open (every 1 second). Players see their live stats without needing to reopen the menu.

---

## 🎁 Tutorial 4: Multi-Page Crate Rewards Preview

A paginated rewards preview for server crates. This demonstrates scenes with navigation and decorative borders.

### Step 1: Create the Structure

```yaml
id: craterewards
rows: 6
type: chest
title: §d§lCrate Rewards Preview
commandAlias: rewards
```

### Step 2: Build Page 1

```yaml
scenes:
  '0':
    delay: 0
    open-events:
      page-sound:
        sound: BLOCK_CHEST_OPEN
    items:
      # Header
      '1':
        slot: 4
        item: CHEST
        amount: 1
        item-name: §d§lVote Crate
        item-lore:
          - ''
          - §7Preview of all possible rewards
          - §7Page §e1§7/§e2
          - ''

      # Reward items
      '2':
        slot: 19
        item: DIAMOND
        amount: 5
        item-name: §bDiamonds x5
        item-lore:
          - §7Chance: §a15%
      '3':
        slot: 20
        item: EMERALD
        amount: 3
        item-name: §aEmeralds x3
        item-lore:
          - §7Chance: §a20%
      '4':
        slot: 21
        item: IRON_INGOT
        amount: 16
        item-name: §fIron Ingots x16
        item-lore:
          - §7Chance: §a25%
      '5':
        slot: 22
        item: GOLDEN_APPLE
        amount: 2
        item-name: §6Golden Apples x2
        item-lore:
          - §7Chance: §e10%
      '6':
        slot: 23
        item: EXPERIENCE_BOTTLE
        amount: 10
        item-name: §aXP Bottles x10
        item-lore:
          - §7Chance: §a30%

      # Navigation
      '50':
        slot: 53
        item: ARROW
        amount: 1
        item-name: §eNext Page →
        item-lore:
          - §7Click for more rewards
        click-events:
          next-scene-click: {}
          sound-click-event:
            sound: UI_BUTTON_CLICK

      # Close
      '51':
        slot: 49
        item: BARRIER
        amount: 1
        item-name: §cClose
        click-events:
          close-inventory: {}
```

### Step 3: Build Page 2

```yaml
  '1':
    delay: 0
    open-events:
      page-sound:
        sound: UI_BUTTON_CLICK
    items:
      # Header
      '1':
        slot: 4
        item: CHEST
        amount: 1
        item-name: §d§lVote Crate
        item-lore:
          - ''
          - §7Preview of all possible rewards
          - §7Page §e2§7/§e2
          - ''

      # Rare rewards
      '2':
        slot: 19
        item: NETHERITE_INGOT
        amount: 1
        item-name: §4§lNetherite Ingot
        item-lore:
          - §7Chance: §c1%
          - §6§lLEGENDARY
      '3':
        slot: 20
        item: ELYTRA
        amount: 1
        item-name: §d§lElytra
        item-lore:
          - §7Chance: §c0.5%
          - §6§lLEGENDARY
      '4':
        slot: 21
        item: TOTEM_OF_UNDYING
        amount: 1
        item-name: §e§lTotem of Undying
        item-lore:
          - §7Chance: §c2%
          - §5§lEPIC

      # Navigation
      '50':
        slot: 45
        item: ARROW
        amount: 1
        item-name: §e← Previous Page
        item-lore:
          - §7Go back to page 1
        click-events:
          previous-scene-click: {}
          sound-click-event:
            sound: UI_BUTTON_CLICK

      # Close
      '51':
        slot: 49
        item: BARRIER
        amount: 1
        item-name: §cClose
        click-events:
          close-inventory: {}
```

---

## 🛡 Tutorial 5: Staff Punishment Panel with Logging

An admin tool that combines player pickers, chat fetcher input, and player data to create a punishment system with logging.

### Step 1: The Main Menu

```yaml
id: punish
rows: 3
type: chest
title: §4§lStaff Panel
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
        item-name: §c§lBan Player
        item-lore:
          - ''
          - §7Permanently ban a player
          - §7from the server
          - ''
          - §cClick to select player
        click-events:
          player-picker-by-console-command:
            command: ban %player% Banned by %executor%
          message:
            message: §c%executor% banned %player%
          sound-click-event:
            sound: BLOCK_ANVIL_LAND

      '2':
        slot: 12
        item: IRON_DOOR
        amount: 1
        item-name: §e§lKick Player
        item-lore:
          - ''
          - §7Kick a player from
          - §7the server
          - ''
          - §eClick to select player
        click-events:
          player-picker-by-console-command:
            command: kick %player% Kicked by %executor%
          message:
            message: §e%executor% kicked %player%

      '3':
        slot: 14
        item: WRITABLE_BOOK
        amount: 1
        item-name: §b§lWarn Player
        item-lore:
          - ''
          - §7Issue a warning with
          - §7a custom reason
          - ''
          - §bClick to select player
        click-events:
          player-picker-by-console-command:
            command: warn %player% Warned by %executor%
          message:
            message: §b%executor% warned %player%

      '4':
        slot: 16
        item: LECTERN
        amount: 1
        item-name: §a§lPardon Player
        item-lore:
          - ''
          - §7Unban a player
          - §7(includes offline players)
          - ''
          - §aClick to select player
        click-events:
          offline-player-picker-command:
            command: unban %player%
          message:
            message: §a%executor% pardoned %player%
```

### Step 2: Add Custom Reason via Chat Fetcher

For a ban with a custom reason, you could use a chat fetcher approach:

```yaml
      '5':
        slot: 22
        item: WITHER_SKELETON_SKULL
        amount: 1
        item-name: §4§lBan with Reason
        item-lore:
          - ''
          - §7Ban a player with a
          - §7custom reason message
          - ''
          - §4Click to start
        click-events:
          chat-fetcher:
            message: §eType the player name to ban:
            conditionFailMessage: §cInvalid player name!
            click-events:
              console_command:
                commands:
                  - ban %input% Custom ban by %player%
              message:
                message: §cBanned %input% with custom reason
```

---

## 🚀 What's Next?

- 📦 Browse [Premade Configurations](Premade-Configurations) for ready-to-use GUI templates
- 🧩 Learn about [Plugin Integrations](Plugin-Integrations) for connecting with other plugins
- 💡 Read [Tips & Best Practices](Tips-and-Best-Practices) for optimization advice

---

| ← Previous | Next → |
|:---|---:|
| [**Developer API**](Developer-API) | [**Plugin Integrations**](Plugin-Integrations) |
