# Local install (fork build)

This fork adds one per-workspace setting to Workspaces Plus:
**Keep the current file open when loading this workspace**.
When it is on, the file you were reading stays in the main pane after the
workspace layout is applied. The setting is off for every workspace by default.

Upstream PR: https://github.com/jsmorabito/obsidian-workspaces-plus/pull/146 (branch `feat/keep-active-file`).
This page is for the local build only. It is not part of the PR.

## Build and install

```bash
cd ~/Developer/agent/claude/obsidian-workspaces-plus
git checkout local/keep-active-file
npm i
npm run build
V="$HOME/Library/Mobile Documents/iCloud~md~obsidian/Documents/Note/.obsidian/plugins/workspaces-plus"
mkdir -p "$V"
cp main.js manifest.json styles.css "$V/"
```

Then in Obsidian: Settings → Community plugins → reload the plugin list →
turn on **Workspaces Plus**. The core **Workspaces** plugin must stay on.

## Turn on the option

Settings → Workspaces Plus → section **Workspaces** → open **写卡** →
turn on **Keep the current file open when loading this workspace**.

The value is stored in the vault's `.obsidian/workspaces.json`, under the
workspace's `workspaces-plus:settings-v1` key as `keepActiveFile: true`.
It survives a plugin reinstall and a switch back to the official build.

Related defaults you may want to change in the same settings page:

- **Automatically track and restore open files** is on by default. It saves
  the open files of each workspace on save and restores them on load. A
  layout-only desk does not need it. Turn it off if you only want layouts.
- **Reload workspace layout on startup** is off by default. Leave it off.

## Rebind the hotkeys

Workspaces Plus registers one command per workspace with id
`workspaces-plus:<workspace name>` and the display name `Load: <workspace name>`.

| Desk command (current) | Workspaces Plus command (new) | Hotkey |
|---|---|---|
| `desk:分拣` | `workspaces-plus:分拣` | Cmd+Alt+1 |
| `desk:写卡` | `workspaces-plus:写卡` | Cmd+Alt+2 |
| `desk:研究` | `workspaces-plus:研究` | Cmd+Alt+3 |
| `desk:回顾` | `workspaces-plus:回顾` | Cmd+Alt+4 |
| `desk:成文` | `workspaces-plus:成文` | Cmd+Alt+5 |

Two ways to rebind:

1. In Obsidian: Settings → Hotkeys → search `Load:` → set each one. Then
   remove the five `desk:` bindings, or turn the Desk plugin off so they
   stop firing.
2. Edit `.obsidian/hotkeys.json` and rename the five keys from `desk:` to
   `workspaces-plus:`. Restart Obsidian after the edit.

## Retire Desk

1. Settings → Community plugins → turn off **Desk**.
2. Optional: delete `.obsidian/plugins/desk/`.

## Switch back to the official build

When the upstream release contains this change (watch the PR for the merge
and the release tag):

1. Delete `.obsidian/plugins/workspaces-plus/` from the vault.
2. Settings → Community plugins → Browse → install **Workspaces Plus**.
3. Turn it on. The `keepActiveFile` value in `workspaces.json` is still there.

If the upstream PR is not merged, keep using this fork. To update it:

```bash
git fetch upstream
git checkout local/keep-active-file
git rebase upstream/master
npm run build
```

and copy the three files again.
