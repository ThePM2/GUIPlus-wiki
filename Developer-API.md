> 💻 **FOR DEVELOPERS**

# Developer API

*Create and manage GUIs programmatically with the Java API*

---

GUIPlus exposes its GUI classes, action and condition registries, chat input and player data to Java plugins. These examples target GUIPlus **4.0.0**. Compile against the same plugin JAR that you run on your server and use the server thread for GUI and Bukkit operations.

## 📦 Maven / Local Dependency

Install your purchased GUIPlus JAR into your local Maven repository. Replace the path with the location of your copy:

```shell
mvn install:install-file -Dfile="path/to/GUIPlus-4.0.0.jar" -DgroupId=com.infiniteplugins -DartifactId=GUIPlus -Dversion=4.0.0 -Dpackaging=jar -DgeneratePom=true
```

Then add a provided dependency:

```xml
<dependency>
    <groupId>com.infiniteplugins</groupId>
    <artifactId>GUIPlus</artifactId>
    <version>4.0.0</version>
    <scope>provided</scope>
</dependency>
```

You also need your server API as a provided dependency. GUIPlus's classes require Java 21 or newer; use a compiler new enough for your chosen server API too, such as JDK 25 or newer for Paper 26.x. Do not bundle GUIPlus inside your own plugin. Declare the dependency in your `plugin.yml`:

```yaml
depend: [GUIPlus]
```

Use `softdepend: [GUIPlus]` instead only when the integration is optional, and keep GUIPlus classes out of code paths that run without it.

## 🔌 Getting the Plugin Instance

```java
import com.infiniteplugins.guiplus.GUIPlus;
import com.infiniteplugins.guiplus.api.gui.GuiManager;

GUIPlus plugin = GUIPlus.getInstance();
GuiManager manager = GUIPlus.getGuiManager();
```

Use these after GUIPlus has enabled. A GUIPlus reload replaces loaded GUI objects; look up a GUI by ID again rather than keeping an old reference indefinitely.

## 🖼 Creating a GUI Programmatically

A GUI needs a scene and an item layout before it can display useful content. The following method can be called from your plugin on the server thread:

```java
import com.infiniteplugins.guiplus.GUIPlus;
import com.infiniteplugins.guiplus.api.gui.GUI;
import com.infiniteplugins.guiplus.api.gui.GuiManager;
import com.infiniteplugins.guiplus.api.gui.data.GuiItem;
import com.infiniteplugins.guiplus.api.gui.data.Scene;
import com.infiniteplugins.guiplus.click.impl.MessageClickEvent;
import org.bukkit.Material;
import org.bukkit.entity.Player;
import org.bukkit.inventory.ItemStack;

public void openExample(Player player) {
    GuiManager manager = GUIPlus.getGuiManager();
    GUI gui = manager.getGui("myplugin_menu");

    if (gui == null) {
        gui = manager.createGuiAndAdd("myplugin_menu", GUI.Type.CHEST);
        gui.setTitle("&6My Custom Menu");
        gui.setRows(3);

        Scene scene = gui.addScene();
        scene.setDelay(0L);

        GuiItem button = new GuiItem(new ItemStack(Material.EMERALD), scene);
        button.setDisplayName("&aSay hello");
        button.getLore(1).setLines(java.util.List.of("&7Click for a greeting."));

        MessageClickEvent greeting = new MessageClickEvent(button);
        greeting.setMessage("&aHello from my plugin!");
        button.getClickEvent().add(greeting);
        scene.getItemStorageMap().put(13, button);
    }

    gui.open(player);
}
```

Slots are zero-based. `createGuiAndAdd` rejects a duplicate ID; use an ID belonging to your plugin. Creating/registering a GUI changes the live manager, not a YAML file immediately. GUIPlus's normal save workflow can persist registered GUIs, so use [Creating GUIs](Creating-GUIs.md) when you want a file-authored menu.

## 📦 GUI Types

| Type | Layout |
|---|---|
| `GUI.Type.CHEST` | 1–6 rows of 9 slots |
| `GUI.Type.DISPENSER` | 3×3 slots |
| `GUI.Type.DROPPER` | 3×3 slots |
| `GUI.Type.HOPPER` | 5 slots |

Only chest inventories use the row count.

## 🔍 Accessing and Opening Existing GUIs

```java
GUI gui = GUIPlus.getGuiManager().getGui("shop");
if (gui != null) {
    gui.open(player);
}

java.util.List<GUI> allGuis = GUIPlus.getGuiManager().getAll();
```

For a menu that uses a separate online target, pass that player explicitly:

```java
GUI gui = GUIPlus.getGuiManager().getGui("staff_profile");
if (gui != null) {
    gui.open(viewer, targetPlayer);
}
```

Opening uses GUIPlus's normal permission, scene-admission and session handling. Do not bypass it by opening a raw inventory yourself.

## 🖱 Custom Click Events

Extend `GUIClick` and provide a public constructor accepting `Clickable`. The registry constructs examples with a null owner, so the constructor must tolerate it. This minimal action has no saved parameters:

```java
import com.infiniteplugins.guiplus.api.config.FileConfiguration;
import com.infiniteplugins.guiplus.click.Clickable;
import com.infiniteplugins.guiplus.click.GUIClick;
import com.infiniteplugins.guiplus.utils.ClickableProcess;
import com.infiniteplugins.guiplus.utils.xseries.XMaterial;
import org.bukkit.entity.Player;
import org.bukkit.event.inventory.InventoryClickEvent;

import java.util.function.Consumer;
import java.util.function.Function;

public class MyCustomClickEvent extends GUIClick {
    public MyCustomClickEvent(Clickable owner) {
        super(owner);
    }

    @Override public String getId() { return "myplugin-greeting"; }
    @Override public String getName() { return "My Plugin Greeting"; }
    @Override public String lore() { return "Sends a greeting."; }
    @Override public XMaterial itemMat() { return XMaterial.EMERALD; }

    @Override
    public void onClick(Player player, Player target,
                        InventoryClickEvent event,
                        Function<String, String> modifier,
                        ClickableProcess process) {
        player.sendMessage("Hello from my plugin!");
        process.notifyProcess();
    }

    @Override
    public void runSetup(Player player, Consumer<Player> onComplete) {
        onComplete.accept(player);
    }

    @Override public void save(FileConfiguration.ConfigurationSection section) { }
    @Override public void load(FileConfiguration.ConfigurationSection section) { }
}
```

The distributed GUIPlus JAR relocates XSeries to `com.infiniteplugins.guiplus.utils.xseries`; use that import when compiling against the purchased JAR.

Call `notifyProcess()` once when your action has completed successfully. An interactive action should wait for its callback before continuing. Call `cancel()` when the action fails. Do not call both to signal the same outcome, and do not continue a process that was already cancelled. The modifier may be null; use `modify(process, text)` for configured text that must receive the current input and selected-player substitutions.

## ✅ Custom Conditions

A condition needs a public no-argument constructor. Return the ordinary, non-inverted result; the base class applies `inverted` and treats evaluation failures as a denial.

```java
import com.infiniteplugins.guiplus.api.config.FileConfiguration;
import com.infiniteplugins.guiplus.conditions.GUICondition;
import com.infiniteplugins.guiplus.utils.xseries.XMaterial;
import org.bukkit.entity.Player;
import org.bukkit.event.inventory.InventoryClickEvent;

import java.util.function.Consumer;
import java.util.function.Function;

public class HealthyCondition extends GUICondition {
    public HealthyCondition() { }

    @Override public String getId() { return "myplugin-healthy"; }
    @Override public String getName() { return "More than five hearts"; }
    @Override public String lore() { return "Requires health above 10."; }
    @Override public XMaterial itemMat() { return XMaterial.GOLDEN_APPLE; }

    @Override
    public boolean canDisplayInternal(Player player, Function<String, String> modifier) {
        return player.getHealth() > 10.0;
    }

    @Override
    public void runSetup(Player player, Consumer<Player> onComplete,
                         InventoryClickEvent event) {
        onComplete.accept(player);
    }

    @Override public void save(FileConfiguration.ConfigurationSection section) { }
    @Override public void load(FileConfiguration.ConfigurationSection section) { }
}
```

Keep visibility conditions free of side effects: GUIPlus may evaluate them while rendering. Use item `conditions` for click requirements and `showConditions` for visibility.

## 🧩 Registering Extension Types

Register your classes before GUIPlus reads saved GUIs that use them. For simple types such as those above, register from your plugin's `onLoad()`; constructors and registration must not require an enabled GUIPlus instance:

```java
import com.infiniteplugins.guiplus.click.ClickEventManager;
import com.infiniteplugins.guiplus.conditions.ConditionManager;

@Override
public void onLoad() {
    ClickEventManager.addClickEvent(MyCustomClickEvent.class);
    ConditionManager.addConditionType(HealthyCondition.class);
}
```

Use stable, unique IDs. Once registered, the types appear in the editor and can be used in YAML:

```yaml
conditions:
  myplugin-healthy: {}
click-events:
  greeting:
    type: myplugin-greeting
```

An arbitrary action entry name needs `type`; an arbitrary condition entry name needs `condition`. Unknown or unavailable types prevent the affected GUI from loading. If your integration registers later, reload GUIPlus after registration so those GUIs can be read again. Built-in parameter validation does not validate your custom parameters; validate them in `load` and preserve them in `save`.

## 💾 Player Data API

```java
import com.infiniteplugins.guiplus.data.PlayerData;
import com.infiniteplugins.guiplus.data.PlayerDataManager;

PlayerData data = PlayerDataManager.getPlayerData(player.getUniqueId());
String kills = data.getValue("kills");
String points = data.getValueOrDefault("points", "0");
data.setValue("kills", "25");
```

`PlayerData` is a record backed by the shared player-data configuration. `setValue` commits the update and throws if it cannot commit. To change several fields together and inspect the result:

```java
import java.util.Map;

var result = data.updateValues(Map.of("kills", "25", "points", "100"));
if (!result.committed()) {
    getLogger().warning("Could not save player data: " + result.failure());
}
```

That commit groups those GUIPlus fields. It does not include another plugin's balance, commands or Minecraft player inventory in the same transaction.

## 💬 Chat Fetcher API

Create a prompt on the server thread and call `build()` to register it:

```java
import com.infiniteplugins.guiplus.api.chat.ChatFetcher;

player.sendMessage("Enter a label, or type exit to cancel.");
new ChatFetcher(player)
    .setFilter(input -> !input.isBlank() && input.length() <= 32)
    .setFetcherFilterFailMessage("Please enter 1 to 32 characters.")
    .onFetch(input -> {
        player.sendMessage("Accepted: " + input);
        return true;
    })
    .onExit(p -> p.sendMessage("Input cancelled."))
    .build();
```

`onFetch` returns `true` to finish or `false` to keep waiting. GUIPlus routes chat acceptance callbacks to its synchronous scheduler. `new ChatFetcher(player, true)` suppresses the exit help message; it does not disable cancellation. For typed inputs, the fetcher also accepts a `ChatInputSpec` through `withInputSpec`. See [Input Rules](Input-Rules.md) for the supported types and constraints.

## ⏱ Scheduler Utilities

```java
GUIPlus.sync().run(() -> {
    // GUI and Bukkit operations.
});

GUIPlus.sync().after(20L).run(() -> {
    // About one second later at 20 TPS.
});

GUIPlus.async().run(() -> {
    // Background work that does not access Bukkit's world/player/GUI state.
});
```

Delays use ticks. GUIPlus does not support Folia.

## 📡 Inventory and Session Handling

The public `GuiManager` handles `InventoryClickEvent`, `InventoryCloseEvent` and `InventoryDragEvent` at `LOWEST` priority, and cleans up sessions on `PlayerQuitEvent`. Other plugins may cancel openings or teleports. Use the public opening methods and your own plugin's normal listener lifecycle when adding behavior around a GUI.

---

> **Tip:** Most menus can be built without Java. Start with [Creating GUIs](Creating-GUIs.md), then explore [Shared Components](Shared-Components.md), [Button States](Button-States.md) and [Interaction Flows](Interaction-Flows.md).

---

| ← Previous |
|:---|
| [**FAQ & Troubleshooting**](FAQ-and-Troubleshooting.md) |
