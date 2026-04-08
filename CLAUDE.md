# Theme Creation Guide

This file documents the patterns and rules for creating VS Code color themes from existing Vim/Neovim/Emacs colorschemes.

---

## File Naming and Registration

- Theme file: `themes/<name>.color-theme.json`
- Label in `package.json`: `"Ported - <Name>"` — the prefix groups all themes together under "P" in VS Code's alphabetical theme picker
- Add the entry to `package.json` in alphabetical order within the `contributes.themes` array

```json
{
  "label": "Ported - ThemeName",
  "uiTheme": "vs-dark",
  "path": "./themes/themename.color-theme.json"
}
```

After adding a theme, rebuild the extension:
```bash
npx vsce package
```

---

## Theme File Structure

Every theme file has two top-level sections:

```json
{
  "name": "Ported - ThemeName",
  "type": "dark",
  "colors": { ... },
  "tokenColors": [ ... ]
}
```

- `colors` — VS Code UI colors (editor, sidebar, tabs, status bar, popups, etc.)
- `tokenColors` — syntax highlighting token rules

---

## Extracting Colors from Vim/Neovim Colorschemes

### Reading a Vim colorscheme

Map Vim highlight groups to VS Code equivalents:

| Vim Group | VS Code Usage |
|-----------|--------------|
| `Normal` fg/bg | `editor.foreground` / `editor.background` |
| `Comment` | `comment` token scope |
| `Constant` | `constant.numeric`, `constant.language` |
| `String` | `string` token scope |
| `Identifier` | `variable`, `entity.name.function` |
| `Statement` / `Keyword` | `keyword`, `keyword.control` |
| `PreProc` / `Include` | `keyword.control.import`, `meta.preprocessor` |
| `Type` | `entity.name.type`, `support.type` |
| `Special` | `constant.character`, `support.constant` |
| `LineNr` | `editorLineNumber.foreground` |
| `CursorLine` bg | `editor.lineHighlightBackground` |
| `Visual` bg | `editor.selectionBackground` |
| `Pmenu` bg | use for popup/menu backgrounds (see popup rule below) |
| `StatusLine` | `statusBar.background` / `statusBar.foreground` |
| `Search` bg | `editor.findMatchBackground` |

### Reading a Neovim (Lua) colorscheme

Look for palette tables at the top of the file (e.g., `M.palette = { ... }`). These define named colors that are then applied to highlight groups. Read the palette first, then trace how colors are assigned to groups like `Normal`, `Comment`, `String`, etc.

---

## Required `colors` Keys

Always include all of these — never leave popup/modal/menu surfaces unset or they will default to VS Code's gray.

### Editor
```json
"editor.background": "<bg>",
"editor.foreground": "<fg>",
"editor.lineHighlightBackground": "<slightly lighter than bg>",
"editor.selectionBackground": "<selection color>",
"editor.inactiveSelectionBackground": "<dimmer selection>",
"editorCursor.foreground": "<fg or accent>",
"editorLineNumber.foreground": "<muted>",
"editorLineNumber.activeForeground": "<brighter muted>",
"editorIndentGuide.background1": "<subtle>",
"editorIndentGuide.activeBackground1": "<less subtle>",
"editorWhitespace.foreground": "<very subtle>",
"editorRuler.foreground": "<subtle>"
```

### Activity Bar & Sidebar
```json
"activityBar.background": "<slightly darker than bg>",
"activityBar.foreground": "<fg>",
"activityBar.inactiveForeground": "<muted>",
"activityBarBadge.background": "<accent>",
"activityBarBadge.foreground": "<bg>",
"sideBar.background": "<slightly darker than bg>",
"sideBar.foreground": "<fg>",
"sideBar.border": "<subtle border>",
"sideBarSectionHeader.background": "<bg>",
"sideBarSectionHeader.foreground": "<muted fg>"
```

### Status Bar
```json
"statusBar.background": "<accent or dark color>",
"statusBar.foreground": "<bright fg>",
"statusBar.border": "<same as bg>",
"statusBar.noFolderBackground": "<same as bg>",
"statusBar.debuggingBackground": "<secondary color>",
"statusBar.debuggingForeground": "<fg>",
"statusBarItem.hoverBackground": "<lighter>",
"statusBarItem.remoteBackground": "<accent>",
"statusBarItem.remoteForeground": "<bg>"
```

### Tabs — CRITICAL RULES
```json
"tab.activeBackground": "<MUST match tab.inactiveBackground>",
"tab.activeForeground": "<fg>",
"tab.inactiveBackground": "<tab bar bg, same as editorGroupHeader.tabsBackground>",
"tab.inactiveForeground": "<muted>",
"tab.border": "<same as tab bar bg>",
"tab.activeBorder": "<accent color — this is the BOTTOM border on the active tab>",
"tab.unfocusedActiveBorder": "<muted version of accent>",
"tab.hoverBackground": "<slightly lighter than inactive>",
"tab.hoverForeground": "<fg>",
"editorGroupHeader.tabsBackground": "<slightly darker than editor bg>"
```

**Rules:**
- `tab.activeBackground` MUST equal `tab.inactiveBackground` — no background highlight on active tabs
- `tab.activeBorder` = bottom border only, using the theme's primary accent color
- NEVER set `tab.activeBorderTop` — this adds an unwanted top border

### Popups, Menus, Modals — CRITICAL RULES

**Every popup surface MUST match the editor background exactly.** If they are left unset or set to VS Code defaults, they will appear as a jarring gray that breaks the theme.

```json
"dropdown.background": "<editor.background>",
"dropdown.foreground": "<fg>",
"dropdown.border": "<visible border color>",

"menu.background": "<editor.background>",
"menu.foreground": "<fg>",
"menu.selectionBackground": "<slightly lighter than bg>",
"menu.selectionForeground": "<fg>",
"menu.selectionBorder": "<visible border — MUST NOT match menu.background>",
"menu.separatorBackground": "<slightly lighter than bg>",
"menu.border": "<visible border color>",
"menubar.selectionBackground": "<slightly lighter than bg>",
"menubar.selectionForeground": "<fg>",

"quickInput.background": "<editor.background>",
"quickInput.foreground": "<fg>",
"quickInputList.focusBackground": "<slightly lighter than bg>",
"quickInputTitle.background": "<editor.background>",

"editorWidget.background": "<editor.background>",
"editorWidget.foreground": "<fg>",
"editorWidget.border": "<visible border color>",
"editorSuggestWidget.background": "<editor.background>",
"editorSuggestWidget.foreground": "<fg>",
"editorSuggestWidget.border": "<visible border color>",
"editorSuggestWidget.selectedBackground": "<slightly lighter than bg>",
"editorSuggestWidget.highlightForeground": "<accent>",
"editorHoverWidget.background": "<editor.background>",
"editorHoverWidget.foreground": "<fg>",
"editorHoverWidget.border": "<visible border color>",

"notifications.background": "<editor.background>",
"notifications.foreground": "<fg>",
"notifications.border": "<visible border color>",
"notificationCenterHeader.background": "<editor.background>",
"notificationCenterHeader.foreground": "<fg>"
```

**The border color must always be distinctly visible against the background — not the same color as `menu.background`.**

### Input, Buttons, Badges
```json
"input.background": "<slightly lighter than editor bg>",
"input.foreground": "<fg>",
"input.border": "<subtle border>",
"input.placeholderForeground": "<muted>",
"inputOption.activeBorder": "<accent>",
"button.background": "<accent>",
"button.foreground": "<bg or white>",
"button.hoverBackground": "<lighter accent>",
"badge.background": "<accent>",
"badge.foreground": "<bg>"
```

### Lists
```json
"list.activeSelectionBackground": "<slightly lighter than bg>",
"list.activeSelectionForeground": "<fg>",
"list.inactiveSelectionBackground": "<between bg and active>",
"list.inactiveSelectionForeground": "<fg>",
"list.hoverBackground": "<lineHighlight color>",
"list.focusBackground": "<slightly lighter than bg>",
"list.highlightForeground": "<accent>"
```

### Terminal
Map the terminal ANSI colors to the theme palette:
```json
"terminal.background": "<editor.background>",
"terminal.foreground": "<fg>",
"terminal.ansiBlack": "<bg>",
"terminal.ansiRed": "<red/error color>",
"terminal.ansiGreen": "<green/string color>",
"terminal.ansiYellow": "<yellow/keyword color>",
"terminal.ansiBlue": "<blue/comment color>",
"terminal.ansiMagenta": "<magenta/type color>",
"terminal.ansiCyan": "<cyan/special color>",
"terminal.ansiWhite": "<fg>",
"terminal.ansiBrightBlack": "<muted>",
... (bright variants = slightly brighter versions)
```

### Git Decorations
```json
"gitDecoration.addedResourceForeground": "<green from palette>",
"gitDecoration.modifiedResourceForeground": "<yellow from palette>",
"gitDecoration.deletedResourceForeground": "<red from palette>",
"gitDecoration.untrackedResourceForeground": "<blue/cyan from palette>",
"gitDecoration.ignoredResourceForeground": "<muted>"
```

---

## Token Color Rules

Always include these token scope groups. Apply `"fontStyle": "italic"` to comments, and `"fontStyle": "bold"` to keywords/types when the original scheme uses bold.

```json
"tokenColors": [
  { "name": "Comment", "scope": ["comment", "punctuation.definition.comment"], "settings": { "foreground": "<comment color>", "fontStyle": "italic" } },
  { "name": "String", "scope": ["string", "string.quoted"], "settings": { "foreground": "<string color>" } },
  { "name": "String Escape", "scope": ["constant.character.escape", "string.regexp"], "settings": { "foreground": "<special color>" } },
  { "name": "Number", "scope": ["constant.numeric"], "settings": { "foreground": "<number color>" } },
  { "name": "Boolean / Constant", "scope": ["constant.language", "constant.other"], "settings": { "foreground": "<constant color>" } },
  { "name": "Keyword", "scope": ["keyword", "keyword.control"], "settings": { "foreground": "<keyword color>" } },
  { "name": "Conditional / Repeat", "scope": ["keyword.control.conditional", "keyword.control.loop", "keyword.control.flow"], "settings": { "foreground": "<conditional color>" } },
  { "name": "Operator", "scope": ["keyword.operator"], "settings": { "foreground": "<operator color>" } },
  { "name": "Storage / Modifier", "scope": ["storage", "storage.type", "storage.modifier"], "settings": { "foreground": "<storage color>" } },
  { "name": "Type", "scope": ["entity.name.type", "entity.other.inherited-class", "support.type", "support.class"], "settings": { "foreground": "<type color>" } },
  { "name": "Function / Method", "scope": ["entity.name.function", "meta.function-call", "support.function", "entity.name.method"], "settings": { "foreground": "<function color>" } },
  { "name": "Variable / Identifier", "scope": ["variable", "variable.other", "entity.name.variable"], "settings": { "foreground": "<identifier color>" } },
  { "name": "Parameter", "scope": ["variable.parameter"], "settings": { "foreground": "<fg or muted>" } },
  { "name": "Include / PreProc", "scope": ["keyword.control.import", "keyword.control.include", "keyword.other.using", "meta.preprocessor", "keyword.control.directive"], "settings": { "foreground": "<preproc color>" } },
  { "name": "Class Name", "scope": ["entity.name.class", "entity.name.namespace"], "settings": { "foreground": "<type color>" } },
  { "name": "Tag", "scope": ["entity.name.tag", "meta.tag"], "settings": { "foreground": "<keyword or accent color>" } },
  { "name": "Tag Attribute", "scope": ["entity.other.attribute-name"], "settings": { "foreground": "<identifier color>" } },
  { "name": "Punctuation", "scope": ["punctuation", "punctuation.definition", "punctuation.separator", "punctuation.accessor"], "settings": { "foreground": "<muted fg>" } },
  { "name": "Invalid", "scope": ["invalid", "invalid.illegal"], "settings": { "foreground": "<error color>", "fontStyle": "underline" } }
]
```

---

## Creating Dimmed Variants

When creating a "Dim" variant of a theme (e.g., Eldar → Eldar Dim):

1. Copy the base theme file to `themes/<name>-dim.color-theme.json`
2. Shift all pure-black surfaces up by one step: `#000000` → `#111111`
3. Shift all dark border/separator colors up accordingly: `#111111` → `#1a1a1a`
4. Keep all syntax token colors identical to the base theme
5. Update any colors that use the old bg as their foreground (e.g., badge text, button text, status bar text that references the bg color)
6. Update the theme name and label

The token colors section is always identical between a theme and its dim variant — only the UI `colors` section changes.

---

## Workflow Summary

1. Read the vim/neovim colorscheme file and extract the hex palette
2. Map palette colors to semantic roles (bg, fg, accent, comment, string, keyword, type, etc.)
3. Create `themes/<name>.color-theme.json` with the full `colors` + `tokenColors` structure above
4. Add the entry to `package.json` in alphabetical order under `contributes.themes`
5. Run `npx vsce package` to rebuild the `.vsix`
6. Install via `Extensions: Install from VSIX...` in VS Code to test

**Do not use agents or subprocesses for color math or theme building — just read the palette directly and build the JSON.**
