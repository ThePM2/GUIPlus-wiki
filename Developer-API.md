> 💻 **FOR DEVELOPERS**

# Developer API

*Create and manage GUIs programmatically with the Java API*

---

GUIPlus provides a public API for developers to create and manage GUIs programmatically, register custom click events and conditions, and interact with the player data system.

> **Note:** The GUIPlus API is designed around direct class usage. Since GUIPlus does not publish to a Maven repository, you will need to add the plugin JAR as a local dependency.

## Getting the Plugin Instance

```java
import com.infiniteplugins.guiplus.GUIPlus;

GUIPlus plugin = GUIPlus.getInstance();
```

## Creating a GUI Programmatically

```java
import com.infiniteplugins.guiplus.api.gui.GUI;
import com.infiniteplugins.guiplus.api.gui.data.Scene;
import com.infiniteplugins.guiplus.api.gui.data.GuiItem;

// Create a new chest GUI
GUI gui = new GUI("myCustomGui", GUI.Type.CHEST);
gui.setTitle("&6My Custom Menu");
gui.setRows(3);

// Register the GUI with the manager
GUIPlus.getGuiManager().addGui(gui);
```

## GUI Types

The `GUI.Type` enum provides these inventory types:

```java
GUI.Type.CHEST      // Standard chest (1-6 rows)
GUI.Type.DISPENSER  // 3x3 dispenser
GUI.Type.DROPPER    // 3x3 dropper
GUI.Type.HOPPER     // 5-slot hopper
```

## Accessing Existing GUIs

```java
import com.infiniteplugins.guiplus.api.gui.GuiManager;

// Get the GUI manager
GuiManager manager = GUIPlus.getGuiManager();

// Get a specific GUI by ID
GUI gui = manager.getGui("bank");

// Get all registered GUIs
List<GUI> allGuis = manager.getAll();

// Check if a GUI exists
boolean exists = manager.getGui("myGui") != null;
```

## Opening a GUI for a Player

```java
Player player = ...;
GUI gui = GUIPlus.getGuiManager().getGui("shop");
if (gui != null) {
    // Open for the player themselves
    gui.open(player);

    // Or open with a target player (e.g., for admin commands)
    gui.open(player, targetPlayer);
}
```

## Custom Click Events

Create custom click event types by extending the `GUIClick` class:

```java
import com.infiniteplugins.guiplus.click.GUIClick;
import com.infiniteplugins.guiplus.click.Clickable;
import com.infiniteplugins.guiplus.utils.ClickableProcess;
import com.cryptomorin.xseries.XMaterial;
import org.bukkit.entity.Player;
import org.bukkit.event.inventory.InventoryClickEvent;
import org.jetbrains.annotations.NotNull;
import org.jetbrains.annotations.Nullable;

import java.util.function.Consumer;
import java.util.function.Function;

public class MyCustomClickEvent extends GUIClick {

    public MyCustomClickEvent(Clickable clickable) {
        super(clickable);
    }

    @Override
    public String getId() {
        return "my-custom-click";
    }

    @Override
    public String getName() {
        return "My Custom Click";
    }

    @Override
    public String lore() {
        return "Does something custom";
    }

    @Override
    public XMaterial itemMat() {
        return XMaterial.REDSTONE;
    }

    @Override
    public void onClick(Player player, @NotNull Player target,
                        InventoryClickEvent event,
                        @Nullable Function<String, String> modifier,
                        @NotNull ClickableProcess clickableProcess) {
        // Your custom logic here
        player.sendMessage("Custom click event fired!");

        // IMPORTANT: Always call this when done to allow subsequent events to proceed
        clickableProcess.notifyProcess();
    }

    @Override
    public void runSetup(Player player, Consumer<Player> onComplete) {
        // Setup wizard logic for the in-game editor
        // Call onComplete when setup is finished
        onComplete.accept(player);
    }

    @Override
    public void save(com.infiniteplugins.guiplus.api.config.FileConfiguration.ConfigurationSection section) {
        // Save your custom properties to the YAML section
    }

    @Override
    public void load(com.infiniteplugins.guiplus.api.config.FileConfiguration.ConfigurationSection section) {
        // Load your custom properties from the YAML section
    }
}
```

> **Important:** Always call `clickableProcess.notifyProcess()` at the end of your `onClick` method. This signals the event processing pipeline to proceed with the next click event in the sequence.

## Custom Conditions

Create custom condition types by extending the `GUICondition` class:

```java
import com.infiniteplugins.guiplus.conditions.GUICondition;
import com.cryptomorin.xseries.XMaterial;
import org.bukkit.entity.Player;
import org.bukkit.event.inventory.InventoryClickEvent;

import java.util.function.Consumer;
import java.util.function.Function;

public class MyCustomCondition extends GUICondition {

    @Override
    public String getId() {
        return "my-custom-condition";
    }

    @Override
    public String getName() {
        return "My Custom Condition";
    }

    @Override
    public String lore() {
        return "Checks something custom";
    }

    @Override
    public XMaterial itemMat() {
        return XMaterial.BARRIER;
    }

    @Override
    public boolean canDisplayInternal(Player player, Function<String, String> modifier) {
        // Return true if the condition passes (item should be shown)
        return player.getHealth() > 10;
    }

    @Override
    public void runSetup(Player player, Consumer<Player> onComplete, InventoryClickEvent event) {
        // Setup wizard logic for the in-game editor
        onComplete.accept(player);
    }

    @Override
    public void save(com.infiniteplugins.guiplus.api.config.FileConfiguration.ConfigurationSection section) {
        // Save your custom properties
    }

    @Override
    public void load(com.infiniteplugins.guiplus.api.config.FileConfiguration.ConfigurationSection section) {
        // Load your custom properties
    }
}
```

> **Note:** The `inverted` property is handled automatically by the base class. Your `canDisplayInternal` should return the non-inverted result.

## Player Data API

Access the persistent player data system:

```java
import com.infiniteplugins.guiplus.data.PlayerDataManager;
import com.infiniteplugins.guiplus.data.PlayerData;

// Get a player's data record
PlayerData data = PlayerDataManager.getPlayerData(player.getUniqueId());

// Read a stored field (returns null if not set)
String kills = data.getValue("kills");

// Read with a default value
String points = data.getValueOrDefault("points", "0");

// Save a field
data.setValue("kills", "25");
```

> **Note:** `PlayerData` is a Java record. Each call to `getPlayerData()` creates a new instance, but all instances share the same underlying YAML config. Changes via `setValue` are saved to disk immediately.

## Chat Fetcher API

Programmatically prompt a player for chat input:

```java
import com.infiniteplugins.guiplus.api.chat.ChatFetcher;

// Basic usage
ChatFetcher fetcher = new ChatFetcher(player.getUniqueId());
fetcher.onFetch(input -> {
    player.sendMessage("You typed: " + input);
    return true; // Return true to accept the input, false to reject
}).build();

// With input filtering and exit handling
new ChatFetcher(player, true)  // true = disable the "type exit to cancel" message
    .setFilter(input -> input.length() > 0)  // Optional pre-filter
    .setFetcherFilterFailMessage("&cInput cannot be empty!")
    .onFetch(input -> {
        player.sendMessage("Accepted: " + input);
        return true;
    })
    .onExit(p -> {
        p.sendMessage("&7Cancelled input.");
    })
    .build();
```

> **Important:** You must call `.build()` at the end to register the chat fetcher with the manager. The `onFetch` callback must return a `Boolean` — `true` to accept the input and complete, `false` to reject and keep waiting.

## Scheduler Utilities

GUIPlus provides a scheduler wrapper for running tasks:

```java
// Run async task
GUIPlus.async().run(() -> {
    // Heavy work off the main thread
});

// Run sync task
GUIPlus.sync().run(() -> {
    // Bukkit API calls on the main thread
});

// Run with delay (in ticks, 20 ticks = 1 second)
GUIPlus.sync().after(20L).run(() -> {
    // Runs 1 second later on the main thread
});
```

## Event Listeners

GUIPlus registers the following Bukkit events in `GuiManager`:

| Event | Priority | Purpose |
|-------|----------|---------|
| `InventoryClickEvent` | LOWEST | GUI item click handling |
| `InventoryCloseEvent` | LOWEST | GUI close handling and cleanup |

> **Note:** `InventoryDragEvent` is handled separately in the internal `SystemGuiManager` for the editor, not in the public `GuiManager`.

## Maven / Local Dependency

Since GUIPlus is not published to a remote Maven repository, add the plugin JAR as a local dependency:

```xml
<dependency>
    <groupId>com.infiniteplugins</groupId>
    <artifactId>GUIPlus</artifactId>
    <version>3.4.10</version>
    <scope>provided</scope>
</dependency>
```

Add `GUIPlus` to your `plugin.yml` dependencies:

```yaml
depend: [GUIPlus]
# or if GUIPlus features are optional:
softdepend: [GUIPlus]
```

---

> **Tip:** Looking for YAML-based configuration instead of the Java API? Most GUIPlus features can be achieved entirely through YAML files. See [Creating GUIs](Creating-GUIs) for the full YAML reference.

---

| ← Previous | Next → |
|:---|---:|
| [**FAQ & Troubleshooting**](FAQ-and-Troubleshooting) | [**Tutorials**](Tutorials) |
