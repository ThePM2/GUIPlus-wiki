> 🎨 **CREATING GUIS**

# Shared Components

*Reuse one item definition across menus with explicit local overrides*

---

A shared component is a reusable item template. Link several menu items to one template, then review the affected uses before publishing a change. Ordinary unlinked items and independent copies keep their own definitions.

## 🛠 Create a Component

1. Hold the item you want to use and open `/gui components`.
2. Choose **Create from held item** and enter a new component ID, such as `help_card`.
3. Open **Edit component template** to change its appearance, metadata, actions and requirements.
4. Add **Shared parameter defaults** for text reused by its instances.
5. Choose **Review and Apply**, inspect the changes and affected uses, then confirm.

This requires `guiplus.components.admin`, granted to operators by default, plus the configured root permission for `/gui`.

You can also choose **Shared component** in an ordinary item editor and then **Create component from this item** to copy an existing menu item's complete definition. Creating a template does not link that item automatically.

The editor works on a detached draft. Changes in sub-editors remain in that draft until the whole component is reviewed and applied. **Cancel whole draft** or closing discards it.

## 🔗 Link a Menu Item

1. Edit an ordinary GUI item and open **Shared component**. On an older customized layout, **Shift-left-click Nested Items** opens the same screen.
2. Choose **Choose shared component** and select the template.
3. Keep **Use as live link**, or choose **Use as independent copy**.
4. Set any **Parameter overrides**, **Local name override** or **Local lore override**.
5. Choose **Review instance and Apply**, review the resolved item and confirm.
6. Finish with the ordinary **Save Gui** action.

| Choice | Result |
|---|---|
| **Live link** | Uses the shared appearance and behavior; future accepted template changes update it. |
| **Local overrides** | Change only this instance's parameters, display name or lore. |
| **Independent copy / Detach** | Materializes the currently resolved item and metadata; later template changes do not update it. |

Linking replaces the item's effective shared appearance and behavior. Old fallback actions are not merged into the template. A linked item opens its link/override panel; detach it before editing its definition independently.

Use `inherit` in a local name or lore prompt to restore inheritance. For local lore, `clear` means an explicitly empty lore override. **Reset all local overrides** restores the shared name, lore and parameter defaults in the pending draft.

## 🏷 Parameters and a YAML Link

Templates use `%component:key%` in supported structured text. Instance parameter values override shared defaults. Local name and lore overrides are separate controls and can also use the component's parameters.

For example, create a `help_card` component with this display name in its template:

```text
&b%component:label%
```

Add a shared parameter default `label=Help`. Then place this item fragment under `scenes.<scene-id>.items` in an existing three-row menu to give one linked instance its own label:

```yaml
'13':
  slot: 13
  item: BOOK
  amount: 1
  shared-component:
    id: 'help_card'
    parameters:
      label: 'Server help'
    lore-override:
      - '&7Read the rules before you begin.'
```

The `help_card` template must already exist. The outer item retains compatibility fields, while the live component supplies its effective definition. Add `name-override: '&eMy help card'` inside `shared-component` for an explicit local display name. Omit an override to inherit; use `lore-override: []` for explicitly empty lore.

Component IDs use **1–48 lowercase letters, digits, `_` or `-`**, starting with a letter. The catalog supports **256 components**. Each component scope supports **32 parameters** with names of **1–32 characters** in the same lowercase format. Values must be **strings of up to 256 characters**, without control characters or nested component tokens. Quote numeric or boolean-looking YAML values, such as `'100'` or `'true'`.

Missing parameters or unsupported bindings reject the definition. Parameter replacement applies to supported structured names, lore and behavior text; it does not rewrite opaque item metadata or turn material, amount and timing fields into text fields.

## 📦 Copying Items and Existing Definitions

Appearance-copy controls copy the cursor item on left-click if one is present, otherwise the main-hand item. Right-click temporarily closes the menu so you can select a different hotbar item and type `copy`; `exit` returns without changing it. Your original item is never consumed.

In virtual item-requirement or cost lists, right-click clears the selected draft slot; Shift-left-click lets you choose another held item.

The component manager edits the template's base appearance, actions and conditions. Existing state, flow and nested definitions can be copied from an ordinary item and preserved. To author states or nested layouts, use the ordinary item editor before copying the definition. A component template cannot contain another shared-component reference; detach such references first.

## 🔄 Revisions and Saved Files

Components and their preceding accepted revisions are stored in `plugins/GUIPlus/SharedComponents.yml`. **Restore previous revision** opens that version as a new draft. Review and confirm it before replacing the current template; restoring a definition cannot undo gameplay actions that already ran.

Another accepted revision or an external file change invalidates an older draft. If an expired screen remains visible, close it and reopen the editor; its controls cannot save the old draft.

Changing a component or its instance retires obsolete views, delayed component actions and component-owned prompts. Reopen the affected menu before using its changed behavior. A missing component cannot execute its fallback actions. If a reload rejects the catalog, the last accepted catalog stays active; an unreadable startup catalog leaves shared items unavailable while unrelated menus can still load.

Back up the component catalog and linked menu files together. Menus saved with shared links require a version that supports those definitions. When returning to an older version, restore the matching backed-up data as well as the JAR.

For shared card and navigation appearance in the automatic EssentialsX browser, see [Warp Catalogs](Warp-Catalogs.md). Those bindings use appearance only; the catalog manages their actions.

---

| ← Previous | Next → |
|:---|---:|
| [**Ready-to-Use Rewards**](Ready-to-Use-Rewards.md) | [**Warp Catalogs**](Warp-Catalogs.md) |
