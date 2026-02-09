> 📦 **RESOURCES**

# Premade Configurations

*Ready-to-use GUI templates for common server needs*

---

These premade configurations are complete, working GUIs that you can drop directly into your `plugins/GUIPlus/CustomGuis/` folder. Customize them to fit your server's theme and requirements.

## 📥 How to Use

1. Copy the YAML below into a new `.yml` file inside `plugins/GUIPlus/CustomGuis/`
2. Run `/gui reload`
3. Open with the listed command or `/gui open <id>`
4. Customize materials, text, commands, and permissions to fit your server

---

## 📜 Server Rules Menu

**Command:** `/rules` | **Type:** Paginated chest GUI

A two-page rules display with navigation. Perfect for showing server rules to new players.

```yaml
id: rules
rows: 5
type: chest
title: §c§lServer Rules
commandAlias: rules
scenes:
  '0':
    delay: 0
    items:
      '1':
        slot: 4
        item: WRITABLE_BOOK
        amount: 1
        item-name: §c§lServer Rules
        item-lore:
          - ''
          - §7Please read all rules carefully
          - §7Ignorance is not an excuse!
          - ''
          - §7Page §e1§7/§e2
          - ''
      '2':
        slot: 19
        item: LIME_CONCRETE
        amount: 1
        item-name: §a§l1. §fBe Respectful
        item-lore:
          - ''
          - §7Treat all players with respect.
          - §7No harassment, hate speech,
          - §7or discrimination of any kind.
          - ''
      '3':
        slot: 21
        item: LIME_CONCRETE
        amount: 2
        item-name: §a§l2. §fNo Cheating
        item-lore:
          - ''
          - §7No hacked clients, x-ray,
          - §7auto-clickers, or any other
          - §7unfair advantage.
          - ''
      '4':
        slot: 23
        item: LIME_CONCRETE
        amount: 3
        item-name: §a§l3. §fNo Griefing
        item-lore:
          - ''
          - §7Do not destroy or modify
          - §7other players' builds without
          - §7their permission.
          - ''
      '5':
        slot: 25
        item: LIME_CONCRETE
        amount: 4
        item-name: §a§l4. §fNo Spamming
        item-lore:
          - ''
          - §7Do not spam chat, commands,
          - §7or any other server feature.
          - ''
      '50':
        slot: 44
        item: ARROW
        amount: 1
        item-name: §eNext Page →
        click-events:
          next-scene-click: {}
          sound-click-event:
            sound: UI_BUTTON_CLICK
      '51':
        slot: 40
        item: BARRIER
        amount: 1
        item-name: §cClose
        click-events:
          close-inventory: {}
  '1':
    delay: 0
    items:
      '1':
        slot: 4
        item: WRITABLE_BOOK
        amount: 1
        item-name: §c§lServer Rules
        item-lore:
          - ''
          - §7Page §e2§7/§e2
          - ''
      '2':
        slot: 19
        item: LIME_CONCRETE
        amount: 5
        item-name: §a§l5. §fNo Advertising
        item-lore:
          - ''
          - §7Do not advertise other servers,
          - §7websites, or services.
          - ''
      '3':
        slot: 21
        item: LIME_CONCRETE
        amount: 6
        item-name: §a§l6. §fReport Bugs
        item-lore:
          - ''
          - §7Report any bugs or exploits
          - §7to staff. Do not abuse them.
          - ''
      '4':
        slot: 23
        item: LIME_CONCRETE
        amount: 7
        item-name: §a§l7. §fHave Fun!
        item-lore:
          - ''
          - §7Most importantly, enjoy
          - §7your time on the server!
          - ''
      '50':
        slot: 36
        item: ARROW
        amount: 1
        item-name: §e← Previous Page
        click-events:
          previous-scene-click: {}
          sound-click-event:
            sound: UI_BUTTON_CLICK
      '51':
        slot: 40
        item: BARRIER
        amount: 1
        item-name: §cClose
        click-events:
          close-inventory: {}
```

---

## 🎮 Gamemode Selector (Staff)

**Command:** `/gm` | **Permission:** `staff.gamemode`

Quick gamemode switcher for staff members.

```yaml
id: gamemode
rows: 1
type: chest
title: §b§lGamemode Selector
commandAlias: gm
permission: staff.gamemode
scenes:
  '0':
    delay: 0
    items:
      '1':
        slot: 1
        item: GRASS_BLOCK
        amount: 1
        item-name: §a§lSurvival
        item-lore:
          - ''
          - §7Switch to Survival mode
          - ''
        click-events:
          command:
            commands:
              - gamemode survival
          sound-click-event:
            sound: ENTITY_PLAYER_LEVELUP
          close-inventory: {}
      '2':
        slot: 3
        item: BRICKS
        amount: 1
        item-name: §6§lCreative
        item-lore:
          - ''
          - §7Switch to Creative mode
          - ''
        click-events:
          command:
            commands:
              - gamemode creative
          sound-click-event:
            sound: ENTITY_PLAYER_LEVELUP
          close-inventory: {}
      '3':
        slot: 5
        item: MAP
        amount: 1
        item-name: §b§lAdventure
        item-lore:
          - ''
          - §7Switch to Adventure mode
          - ''
        click-events:
          command:
            commands:
              - gamemode adventure
          sound-click-event:
            sound: ENTITY_PLAYER_LEVELUP
          close-inventory: {}
      '4':
        slot: 7
        item: ENDER_EYE
        amount: 1
        item-name: §7§lSpectator
        item-lore:
          - ''
          - §7Switch to Spectator mode
          - ''
        click-events:
          command:
            commands:
              - gamemode spectator
          sound-click-event:
            sound: ENTITY_PLAYER_LEVELUP
          close-inventory: {}
```

---

## ✅ Confirmation Dialog

**Usage:** Open with `/gui open confirm` from other click events

A reusable yes/no confirmation dialog. Pair this with other GUIs by opening it via `command` click events.

```yaml
id: confirm
rows: 3
type: chest
title: §e§lAre you sure?
scenes:
  '0':
    delay: 0
    items:
      '1':
        slot: 4
        item: PAPER
        amount: 1
        item-name: §e§lConfirmation Required
        item-lore:
          - ''
          - §7Please confirm your action
          - ''
      '2':
        slot: 11
        item: LIME_CONCRETE
        amount: 1
        item-name: §a§lConfirm
        item-lore:
          - ''
          - §7Click to confirm
          - ''
        click-events:
          message:
            message: §aAction confirmed!
          sound-click-event:
            sound: ENTITY_PLAYER_LEVELUP
          close-inventory: {}
      '3':
        slot: 15
        item: RED_CONCRETE
        amount: 1
        item-name: §c§lCancel
        item-lore:
          - ''
          - §7Click to cancel
          - ''
        click-events:
          message:
            message: §7Action cancelled.
          sound-click-event:
            sound: UI_BUTTON_CLICK
          close-inventory: {}
```

> **Tip:** Customize this template for each use case by changing the title, lore text, and the confirm button's click events.

---

## 🔗 Social Links Menu

**Command:** `/social` | **Type:** Hopper GUI

A compact menu with clickable social media links and server information.

```yaml
id: social
rows: 1
type: hopper
title: §d§lSocial Links
commandAlias: social
scenes:
  '0':
    delay: 0
    items:
      '1':
        slot: 0
        item: PLAYER_HEAD
        amount: 1
        item-name: §9§lDiscord
        item-lore:
          - ''
          - §7Join our community!
          - §9discord.gg/yourserver
          - ''
          - §eClick for invite link
        skullBase64: eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvNzg3M2MxMmJmZmI1MjUxYTBiODhkNWFlNzVjNzI0N2NiMWNkNWYxZjBiOTc4MWUzNTcxOTUzYWQzMTRhMTMifX19
        click-events:
          message:
            message: §9Discord: §nhttps://discord.gg/yourserver
      '2':
        slot: 1
        item: PLAYER_HEAD
        amount: 1
        item-name: §f§lWebsite
        item-lore:
          - ''
          - §7Visit our website
          - §fwww.yourserver.com
          - ''
        skullBase64: eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvZWI0MmM2YjlkZTkzNjJhMGIzMzhmNjgzZmVmMjNlMTRkMmU3N2E1OTFiOTI2YmIyODFjNDlmZDc1YTE0MCJ9fX0=
        click-events:
          message:
            message: §fWebsite: §nwww.yourserver.com
      '3':
        slot: 2
        item: PLAYER_HEAD
        amount: 1
        item-name: §e§lStore
        item-lore:
          - ''
          - §7Support the server!
          - §estore.yourserver.com
          - ''
        skullBase64: eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvNDViOTdiMzVhMjI1NjQ4NTk4OTI2MmI3OWRhOGUzNjBiNTMxODcyMjljMWY1ZjcxYzQzN2M4Y2Y5YmM2In19fQ==
        click-events:
          message:
            message: §eStore: §nstore.yourserver.com
      '4':
        slot: 3
        item: PLAYER_HEAD
        amount: 1
        item-name: §5§lVote
        item-lore:
          - ''
          - §7Vote daily for rewards!
          - ''
        skullBase64: eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvNWRhMDQ3NThhNTE1OGFjNjQ3OGE2N2MxMzE1ZjRmZTRhMGIzN2UxNjE4Y2FkMTRjNmU3MTRhZWZhODYxIn19fQ==
        click-events:
          command:
            commands:
              - vote
      '5':
        slot: 4
        item: PLAYER_HEAD
        amount: 1
        item-name: §a§lMap
        item-lore:
          - ''
          - §7View the live server map
          - §amap.yourserver.com
          - ''
        skullBase64: eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvYzY2OTJmZGRlMjYwMzg0ZThjMGVjMTI4NGM1MjRiMGE2MjE5ZDRhMGI1MjE2NjE0OTc5MjQxNTdmNGM0In19fQ==
        click-events:
          message:
            message: §aMap: §nmap.yourserver.com
```

---

## 🎁 Daily Reward Claim

**Command:** `/daily` | **Type:** Chest GUI with cooldown

A daily reward system using cooldown conditions and player data to track claims.

```yaml
id: daily
rows: 3
type: chest
title: §6§lDaily Rewards
commandAlias: daily
scenes:
  '0':
    delay: 0
    items:
      '1':
        slot: 4
        item: CLOCK
        amount: 1
        item-name: §6§lDaily Rewards
        item-lore:
          - ''
          - §7Claim your daily reward!
          - §7Resets every 24 hours
          - ''

      # Claimable reward (visible when cooldown has passed)
      '2':
        slot: 13
        item: CHEST
        amount: 1
        item-name: §a§lClaim Reward!
        item-lore:
          - ''
          - §7Click to claim your
          - §7daily reward!
          - ''
          - §a§lREADY TO CLAIM
        conditions:
          cooldown:
            cooldown: 86400000
            id: daily-reward
        click-events:
          console_command:
            commands:
              - give %player% diamond 3
              - eco give %player% 1000
          save-player-info:
            save-format: daily-claims:%GUIPlus_player_info_daily-claims%+1
          message:
            message: §6§lDaily Reward Claimed! §7(+3 Diamonds, +$1,000)
          sound-click-event:
            sound: ENTITY_PLAYER_LEVELUP
            volume: 1.0
            pitch: 1.0
          close-inventory: {}

      # Info item
      '3':
        slot: 22
        item: PAPER
        amount: 1
        item-name: §7§lReward Info
        item-lore:
          - ''
          - §7Total Claims: §e%GUIPlus_player_info_daily-claims%
          - ''
          - §7Rewards include:
          - §f  3x Diamond
          - §f  $1,000
          - ''
```

---

## 🧭 Quick Teleport Menu

**Command:** `/tp-menu` | **Type:** Dispenser GUI

A compact 3x3 teleport menu using the dispenser layout.

```yaml
id: tpmenu
rows: 1
type: dispenser
title: §3§lQuick Teleport
commandAlias: tp-menu
scenes:
  '0':
    delay: 0
    items:
      '1':
        slot: 0
        item: RED_BED
        amount: 1
        item-name: §c§lHome
        item-lore:
          - §7Teleport home
        click-events:
          command:
            commands:
              - home
          close-inventory: {}
      '2':
        slot: 1
        item: COMPASS
        amount: 1
        item-name: §a§lSpawn
        item-lore:
          - §7Return to spawn
        click-events:
          command:
            commands:
              - spawn
          close-inventory: {}
      '3':
        slot: 2
        item: ENDER_PEARL
        amount: 1
        item-name: §5§lBack
        item-lore:
          - §7Previous location
        click-events:
          command:
            commands:
              - back
          close-inventory: {}
      '4':
        slot: 3
        item: GRASS_BLOCK
        amount: 1
        item-name: §2§lOverworld
        item-lore:
          - §7Overworld spawn
        click-events:
          teleport:
            location: world
          close-inventory: {}
      '5':
        slot: 4
        item: NETHERRACK
        amount: 1
        item-name: §4§lNether
        item-lore:
          - §7Nether spawn
        click-events:
          teleport:
            location: world_nether
          close-inventory: {}
      '6':
        slot: 5
        item: END_STONE
        amount: 1
        item-name: §e§lThe End
        item-lore:
          - §7End spawn
        click-events:
          teleport:
            location: world_the_end
          close-inventory: {}
      '7':
        slot: 8
        item: BARRIER
        amount: 1
        item-name: §cClose
        click-events:
          close-inventory: {}
```

---

## ✏ Customization Tips

When adapting these templates:

- Replace placeholder commands (e.g., `spawn`, `warp mines`) with your actual server commands
- Update permission nodes to match your permissions setup
- Change materials and colors to fit your server's visual theme
- Add or remove items as needed
- Combine templates — use the warp menu items inside a larger main menu

---

| ← Previous | Next → |
|:---|---:|
| [**Tutorials**](Tutorials.md) | [**Tips & Best Practices**](Tips-and-Best-Practices.md) |
