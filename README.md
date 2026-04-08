# Ported Themes

A collection of VS Code color themes ported from Vim/Neovim colorschemes that didn't have existing VS Code equivalents in the marketplace.

Every theme has been hand-crafted to match the original colorscheme as closely as possible, including full UI coverage — editor, sidebar, tabs, status bar, popups, dropdowns, modals, terminals, and diff views.

## Themes

| Theme | Original Source |
|-------|----------------|
| Ported - Bamboo | [ribru17/bamboo.nvim](https://github.com/ribru17/bamboo.nvim) |
| Ported - Boo | [boo.nvim](https://github.com/rockerBOO/boo-colorscheme-nvim) |
| Ported - Citruszest | [zootedb0t/citruszest.nvim](https://github.com/zootedb0t/citruszest.nvim) |
| Ported - CyberDream | [scottmckendry/cyberdream.nvim](https://github.com/scottmckendry/cyberdream.nvim) |
| Ported - Desertink | [toupeira/desertink.vim](https://github.com/toupeira/desertink.vim) |
| Ported - Eldar | [agude/vim-eldar](https://github.com/agude/vim-eldar) |
| Ported - Eldar Dim | Dimmed variant of Eldar |
| Ported - Elrond | [elrond theme](https://vimcolorschemes.com) |
| Ported - Elrond Dim | Dimmed variant of Elrond |
| Ported - Open Color | [vim-open-color](https://github.com/yrickwong/vim-open-color) |
| Ported - PaperColor | [NLKNguyen/papercolor-theme](https://github.com/NLKNguyen/papercolor-theme) |
| Ported - Paper Color Slim | [vim-colors-paper](https://github.com/programble/vim-colors-paper) |
| Ported - Sprinkles | sprinkles.vim |
| Ported - TangoTango | [gkeep/iceberg-dark](https://github.com/vim-scripts/tangotango) |
| Ported - TangoTango Dim | Dimmed variant of TangoTango |
| Ported - TMNT | tmnt.vim |
| Ported - Wombat | [wombat256.vim](https://github.com/vim-scripts/wombat256.vim) |

## Installation

### From VSIX

1. Download the latest `.vsix` file from the [releases page](https://github.com/CydoEntis/ported-themes/releases)
2. Open VS Code
3. Open the Command Palette (`Ctrl+Shift+P` / `Cmd+Shift+P`)
4. Run `Extensions: Install from VSIX...`
5. Select the downloaded `.vsix` file

### From Source

```bash
git clone https://github.com/CydoEntis/ported-themes
cd ported-themes
npx vsce package
```

Then install the generated `.vsix` as described above.

## License

MIT
