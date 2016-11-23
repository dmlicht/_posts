---
layout: post
title: "Intellij Quickstart"
comments: false
description: "This is a guide for quickly getting productive in Intellij. It's a mix of the notes I took while first learning the tool, and some insights reached after extensive use of the tool. Don't expect much talk of settings twiddling or code coverage tools."
---

This is a guide for quickly getting productive in Intellij. It's a mix of the notes I took while first learning the tool, and some insights reached after extensive use of the tool. Don't expect much talk of settings twiddling or code coverage tools.

First comes first, [Find Action By Name](https://www.jetbrains.com/help/idea/2016.2/navigating-to-action.html) is your new best friend (`⇧⌘A`). If you think there is a function Intellij should have, there is a good chance that it does. **Search Commands** is the quickest way to find and use it. It will also show you the shortcut to quickly jot down and commit to memory.

* `⌘,` opens up preferences, which you can quickly search through by typing
* `⌘;` opens up project preferences

Like using vim bindings? Download [IdeaVim plugin](https://github.com/JetBrains/ideavim), it doesn’t clobber any of the important default shortcuts.

### Favorite Commands:

| Function                                                          | Keys                  |
| ------------------------------------------------------------------|-----------------------|
| Search all commands                                               | `⇧ ⌘ A`               |
| Search project for symbol                                         | `⇧ ⇧`                 |
| Quick Fix problem                                                 | `⌥ Enter`             |
| Open preferences (just start typing to search)                    | `⌘ ,`                 |
| Open Project settings                                             | `⌘ ;`                 |
| Jump to symbol definition                                         | `⌘ B`                 |
| Find all usages of symbol in project                              | `⌥ F7`                |
| Generate (getters/setters/override etc)                           | `⌃ N`                 |
| Jump between split windows                                        | `⌥ tab`               |
| Open quick documentation (w/ vim plugin):                         | `⇧ - k`               |
| Open quick documentation w/out vim plugin:                        | `⌃ j`                 |
| Quick class outline                                               | `⌘ F12`               | 
| Type Hierarchy                                                    | `⇧ ⌃ H`               |
| Method Call Hierarchy                                             | `⌃ alt H`             |
| Hide Tools Windows                                                | `⇧ ⌘ F12`             |
| hotkey to jump to error intellij                                  | `⇧ F2`                |

You can check out a more comprehensive [reference card](http://www.jetbrains.com/idea/docs/IntelliJIDEA_ReferenceCard_Mac.pdf) on the jetbrains website.

### Favorite 3rd Party Plugins:

* [IdeaVim](https://github.com/JetBrains/ideavim)
* [JavaRepl](https://plugins.jetbrains.com/plugin/7215)