---
title: Iterate Tabs in Display Order in Visual Studio Code
tags: [Visual Studio Code, Editor] 
---
Visual Studio Code recently introduced tabs in the [1.3 update][1.3-update]. I was very excited to try it because this was the reason I was still using Sublime Text. However, I was disappointed to find that the default setting is to iterate the tabs in the most recently used order when you hit `Ctrl+Tab` and that a popup list was displayed to choose the tab to display.

Fortunately, you can configure the editor to iterate the tabs in their display order. As a bonus, it also removes the annoying popup.

<!-- more -->

1. Open the keyboard shortcuts configuration (File > Preferences > Keyboard Shortcuts).
2. In the `keybindings.json` file on the right side, add the following bindings:

        // Place your key bindings in this file to overwrite the defaults
        [
          {
            "key": "ctrl+tab",
            "command": "workbench.action.nextEditor"
          },
          {
            "key": "ctrl+shift+tab",
            "command": "workbench.action.previousEditor"
          }
        ]

3. Save the file and enjoy!

[1.3-update]: https://code.visualstudio.com/updates/June_2016