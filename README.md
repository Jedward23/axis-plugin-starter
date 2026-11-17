# Hello Axis: a starter plugin

The smallest useful [Axis](https://tryaxis.so) plugin. Copy it, rename it, and you have a working plugin in a few minutes.

It adds three things to Axis, one for each way a plugin can draw:

| What you see | Slot | How it draws |
| --- | --- | --- |
| A **Hello** section in the project sidebar showing the current project | `sidebar.section` | **Native tree**: Axis components filled from the slot's props |
| A **Say hello** command in ⌘K that types a line into the message box | `palette.command` | **Declarative action**: no code at all |
| A **Hello pane** in the workbench's Add pane menu | `workbench.pane` + `workbench.addMenu` | **Sandboxed frame**: your own HTML in a sealed iframe |

## What's in the folder

```
axis-plugin.json          the manifest: who you are and what you add
hello.html                the pane's HTML (only frame contributions need a file)
axis-plugin.samples.json  sample props, so you can check your bindings without Axis
```

A plugin never touches Axis's code. It fills **slots**, named places Axis draws, and Axis draws what you declare. So your plugin matches Axis in every theme, in light and dark, and on a phone, and it can't slow Axis down or break it. If a contribution throws or renders too slowly, Axis stops it and draws its own view in that spot.

## Make it yours in five minutes

**1. Copy the folder** into any project folder that your Axis can see, for example `plugins/my-plugin/`. On GitHub, use **Use this template**, then clone it there.

**2. Rename it.** In `axis-plugin.json`, change `id` (lowercase, dots and dashes, for example `yourname.my-plugin`), `name`, `publisher` and `description`. Ids that start with `axis.` are reserved.

**3. Change what it draws.**

- **Sidebar section:** edit the `tree`. It must start with a `Stack`; you can use `Stack`, `Row`, `Card`, `Text`, `Badge`, `Metric`, `Callout`, `Table`, `Steps`/`Step`, `BarChart`, `Sparkline` and `Button`. `{ "$bind": "projectPath" }` reads a value from the slot's props by dot path. There are no expressions and no code.
- **Command:** change `title` and `action`. An action is `insert-text`, `open-url` (https only) or `open-pane` (one of your own panes). An action can never send, pay or run code.
- **Pane:** edit `hello.html`. Style it with Axis's theme variables (`var(--ax-text)`, `--ax-accent`, `--ax-muted`, `var(--ax-font)`…) and kit classes (`ax-card`, `ax-stack`, `ax-row`, `ax-badge`, `ax-btn`). `window.axis.props()` and `window.axis.onProps(fn)` give you the slot's props, and `window.axis.open(url)` asks Axis to open an https link after the person confirms. The frame has no network and no storage.

Remove any contribution you don't need. A plugin with only a sidebar section is a valid plugin.

**4. Install it.** In Axis, ask the agent:

> Install the plugin in `plugins/my-plugin`

It calls `axis_plugin_dev_install`, which reads `axis-plugin.json` and the `.html` files it names, checks them with the same validator Axis uses for Community plugins, and lists every problem if there is one. The plugin shows up at once on every device. Edit and install again to update it in place.

**5. See it.** Open a project: the **Hello** section sits under your folders in the sidebar. Press ⌘K (Ctrl K) and type `hello`. In the workbench, choose **Add pane → Hello pane**.

## Turning plugins off: safe mode

Open Axis with `?safe-mode=1` on the address to load it with **no plugins at all**: the plugin code is never even downloaded, and Axis looks exactly as it does with nothing installed. Use it whenever you suspect a plugin. An open plugin pane shows "This pane comes from a plugin that is turned off" instead of your HTML.

To see the full Plugins screen (safe mode switch, each plugin's on/off, and a cost line such as "12 draws · 3.1 ms average"), open Axis with `?developer=on`, then go to **Settings → Advanced → Plugins**.

## Rules Axis enforces

- **Strict manifest.** Unknown slots, fields, element types and props are errors, never ignored. Size limits: 64 K characters per manifest, 20 contributions per slot, trees up to 400 elements and 12 levels deep.
- **Speed budget.** A contribution that renders over 50 ms once, or over 16 ms three times, is stopped for the session and Axis's own view takes its place.
- **Declare what you reach.** `permissions` lists what the plugin can reach (`http:<host>`, `connection:<slot>`, `read:sessions`, …). This one needs none, so it is `[]`.
- **Some screens can't be changed.** Approvals, sign-in, credential cards, install sheets and payments have no slots.

## Other slots

| Slot | Adds |
| --- | --- |
| `transcript.toolResult` | A custom view for a tool's result in chat |
| `space.block` | A block on a Space page (tree or frame) |
| `composer.band` | A compact strip above the message box |
| `agent.status` | A strip under "Working on it…" |
| `agent.game` | Replaces the mini game shown while the agent works |

The `agent.*` slots need `"engines": { "axis": "^1.1.0" }`.

## License

MIT. Copy it, change it, ship it.
