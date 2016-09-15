---
title: Move Lines Up and Down in Visual Studio Code
tags: [Visual Studio Code, Editor] 
---
One feature I find very useful in a text editor is the ability to move a line or a group of lines up or down with a keyboard shortcut. I discovered this possibility in Sublime Text and now I want it in all my editors.

<!-- more -->

Visual Studio Code supports this action with the `Alt+Up` and `Alt+Down` keyboard shorcuts.

## Use the Sublime Text shortcuts

If you are used to Sublime Text and your brain is wired to the `Ctrl+Shift+Up` and `Ctrl+Shift+Down` shortcuts, Visual Studio Code can be configured. Note that those shortcuts are used by default for the `cursorUpSelect` and `cursorDownSelect` commands but those are also accessible from `Shift+Up` and `Shift+Down`.

1. Open the keyboard shortcuts configuration (File > Preferences > Keyboard Shortcuts).
2. In the `keybindings.json` file on the right side, add the following bindings:

        // Place your key bindings in this file to overwrite the defaults
        [
          {
            "key": "ctrl+shift+up",
            "command": "editor.action.moveLinesUpAction",
            "when": "editorTextFocus"
          },
          {
            "key": "ctrl+shift+down",
            "command": "editor.action.moveLinesDownAction",
            "when": "editorTextFocus"
          }
        ]

3. Save the file and enjoy!