---
layout: post
title: "Commands For Vim Macros"
comments: false
description: "These are all the commands you'll need to use vim macros.."
---

In the examples below `<REGISTER>` can be substituted for any character on your keyboard.

| Function | Keys |
|----------|------|
| start recording into `<REGISTER>` | `q<REGISTER>` (ex: `qq`, `qw`, `qe`, `q` + any char)
| end recording | `q` (when currently recording) |
| execute macro saved in `<REGISTER>`  | `@<REGISTER>` (ex: `@q`, `@w`, `@e`) |
| execute macro saved in `<REGISTER>` `<NUMBER>` times | `<NUMBER>@<REGISTER>` |
| repeat last macro | `@@` |
| run macro every time we match the pattern | `:g/<PATTERN>/normal @<REGISTER>` |  
| check the contents of all registers | `:reg(isters)` |  
| check the contents of a given register (this can be chained with several registers, ) | `:reg <REGISTER_ID>` Ex: `qwe` checks register q, w, and e |
| to manually create macros | `let @<REGISTER_ID>=<COMMANDS>` |