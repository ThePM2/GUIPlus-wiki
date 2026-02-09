> 🔌 **INTEGRATIONS**

# DeluxeMenus Converter

*Automatically migrate from DeluxeMenus to GUIPlus*

---

GUIPlus includes a built-in converter that automatically migrates your existing DeluxeMenus configuration files to GUIPlus format.

## How It Works

1. Place your DeluxeMenus GUI files inside the `plugins/GUIPlus/DeluxeMenus/` folder
2. Start or reload the server
3. GUIPlus will automatically detect and convert the files
4. Converted GUIs are output to the `plugins/GUIPlus/CustomGuis/` folder

The converter runs automatically during plugin startup when files are found in the `DeluxeMenus/` folder.

## Re-Converting

If you need to re-run the conversion (e.g., after fixing source files), use:

```
/gui reconvert
```

This requires the `guiplus.gui.reconvert` permission.

## What Gets Converted

The converter handles the core menu structure from DeluxeMenus format:

- Menu title and size
- Item materials and amounts
- Item names and lore
- Basic click commands
- Permission requirements

## After Conversion

After conversion, review your new GUI files in `CustomGuis/` and make any necessary adjustments. You may want to:

- Add GUIPlus-specific features like scenes, chat fetchers, or conditions
- Adjust formatting or add PlaceholderAPI placeholders
- Set up command aliases

## Note

Complex DeluxeMenus features that have no direct equivalent in GUIPlus may need manual adjustment after conversion. Always test your converted GUIs after migration.

> **Note:** The original DeluxeMenus files in `plugins/GUIPlus/DeluxeMenus/` are not modified during conversion. You can safely re-run conversion at any time.

---

| ← Previous | Next → |
|:---|---:|
| [**Plugin Integrations**](Plugin-Integrations) | [**FAQ & Troubleshooting**](FAQ-and-Troubleshooting) |
