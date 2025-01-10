# Visual Studio Code

## Keyboard Shortcuts

- [smartSelect](https://stackoverflow.com/questions/37835012/select-everything-between-matching-brackets-in-vs-code) - used to select content within brackets, braces, quotes
  - Shift + Alt + Right/Left Arrow to expand/contract selection
- Folding - fold regions of source code using the folding icons on the gutter or using keyboard shortcuts. [More Shortcuts Available](https://code.visualstudio.com/docs/editor/codebasics#_folding)
  - Fold (`Ctrl+Shift+[`) folds the innermost uncollapsed region at the cursor.
  - Unfold (`Ctrl+Shift+])` unfolds the collapsed region at the cursor.
  - Unfold All (`Ctrl+K Ctrl+J`) unfolds all regions in the editor.
  - Fold Level X (`Ctrl+K Ctrl+1` for level 1) folds all regions of level X, except the region at the current cursor position.
- Debugging
  - `F9` toggle breakpoint
- CMD+SHIFT+O Navigate between symbols within a file
- shift+cmd+. opens all the methods within classes in file and let's you search

## Tips and Tricks

## Resources
- https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf

## Extensions
- ReWrap (stkb, not signed...)
- GitLens (GitKraken)
- Code Runner (Jun Han)
- Markdown All in One (Yu Zhang)
- MatlabUnofficial (Xavier Hahn)
- Untested
  - [Quick and Simple Text Selection (David Bankier)](https://marketplace.visualstudio.com/items?itemName=dbankier.vscode-quick-select)
  - https://marketplace.visualstudio.com/items?itemName=Gruntfuggly.todo-tree

## Modifications
- https://stackoverflow.com/questions/42796887/switch-focus-between-editor-and-integrated-terminal

## settings.json

```json
{
    "workbench.colorTheme": "Monokai",
    "editor.minimap.enabled": false,
    "diffEditor.renderSideBySide": true,
    "jupyter.askForKernelRestart": false,
    "jupyter.interactiveWindow.creationMode": "perFile",
    "[python]": {
        "editor.formatOnSave": true,
        "editor.codeActionsOnSave": {
          "source.fixAll": "explicit",
          "source.organizeImports": "explicit"
        },
        "editor.defaultFormatter": "charliermarsh.ruff"
      }
}
```
