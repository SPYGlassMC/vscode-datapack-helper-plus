![banner](https://raw.githubusercontent.com/SpyglassMC/vscode-datapack/master/img/banner.png)

[![Discord](https://img.shields.io/discord/666020457568403505?logo=discord&style=flat-square)](https://discord.gg/EbdseuS)
[![GitHub Actions](https://img.shields.io/github/actions/workflow/status/SpyglassMC/Spyglass/commit.yml?branch=main&logo=github&style=flat-square)](https://github.com/SPGoding/vscode-datapack-helper-plus/actions)
[![VSCode Marketplace](https://img.shields.io/visual-studio-marketplace/v/SPGoding.datapack-language-server.svg?logo=visual-studio-code&style=flat-square)](https://marketplace.visualstudio.com/items?itemName=SPGoding.datapack-language-server)
[![VSCode Marketplace Downloads](https://img.shields.io/visual-studio-marketplace/d/SPGoding.datapack-language-server.svg?logo=visual-studio-code&style=flat-square)](https://marketplace.visualstudio.com/items?itemName=SPGoding.datapack-language-server)
[![VSCode Marketplace Installs](https://img.shields.io/visual-studio-marketplace/i/SPGoding.datapack-language-server.svg?logo=visual-studio-code&style=flat-square)](https://marketplace.visualstudio.com/items?itemName=SPGoding.datapack-language-server)
[![VSCode Marketplace Rating](https://img.shields.io/visual-studio-marketplace/stars/SPGoding.datapack-language-server.svg?logo=visual-studio-code&style=flat-square)](https://marketplace.visualstudio.com/items?itemName=SPGoding.datapack-language-server)
[![License](https://img.shields.io/github/license/SpyglassMC/vscode-datapack.svg?style=flat-square)](https://github.com/SPGoding/vscode-datapack-helper-plus/blob/master/LICENSE)
[![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg?style=flat-square)](https://github.com/semantic-release/semantic-release)
[![Gitmoji](https://img.shields.io/badge/gitmoji-%20%F0%9F%98%9C%20%F0%9F%98%8D-FFDD67.svg?style=flat-square)](https://gitmoji.carloscuesta.me/)

#### English / [日本語](https://github.com/ChenCMD/datapack-helper-plus-JP/blob/master/README.md) / [简体中文](https://github.com/SPGoding/vscode-datapack-helper-plus/blob/master/README-zh_Hans.md)

Data-pack Helper Plus can provide many heavy language features for documents in your datapack, including advancements, dimensions, dimension types, functions, loot tables, predicates, recipes, all kinds of tags, and worldgen settings.

If you like this extension, please consider [sponsoring](https://github.com/sponsors/SPGoding) me. You can also report bugs, suggest features, and help translations! See [CONTRIBUTING.md](https://github.com/SPGoding/datapack-language-server/blob/master/CONTRIBUTING.md) for more information.

- [Disclaimer](#disclaimer)
- [Installation](#installation)
- [Features](#features)
   - [Workspace Support](#workspace-support)
   - [Multiple Language Support](#multiple-language-support)
   - [Semantic Highlighting](#semantic-highlighting)
   - [Signature Information](#signature-information)
   - [Completions](#completions)
   - [Code Snippets](#code-snippets)
   - [Declaration Comments](#declaration-comments)
   - [Alias Comments](#alias-comments)
   - [Diagnostics and Code Actions](#diagnostics-and-code-actions)
   - [Formatting and Linting](#formatting-and-linting)
   - [Folding Ranges](#folding-ranges)
   - [Call Hierarchy](#call-hierarchy)
   - [Color Information](#color-information)
   - [Hover Information](#hover-information)
   - [Resolving Namespaced IDs](#resolving-namespaced-ids)
   - [Goto Definitions](#goto-definitions)
   - [Finding References](#finding-references)
   - [Renaming](#renaming)
   - [Configuration Settings](#configuration-settings)
   - [Regenerating Cache](#regenerating-cache)

# Disclaimer

> [MIT License](https://github.com/SPGoding/vscode-datapack-helper-plus/blob/master/LICENSE)

We tried our best to keep your datapacks safe. However, your documents might still be corrupted under certain extreme circumstances by DHP, and we couldn't provide any help if the said thing happened. Please **back up** your precious work from time to time like uploading it to a cloud storage, which is really important even if you choose not to use DHP!

# Installation

You can install DHP from the VSCode Marketplace: [![VSCode Marketplace](https://img.shields.io/visual-studio-marketplace/v/SPGoding.datapack-language-server.svg?logo=visual-studio-code&style=flat-square)](https://marketplace.visualstudio.com/items?itemName=SPGoding.datapack-language-server)

Alternatively, press Ctrl + P and execute `ext install spgoding.datapack-language-server` in your VSCode.

**Note**: DHP has a minimum version requirement of VSCode `1.44.0`. Please make sure that your VSCode is later than this version.

# Features

## Workspace Support

Please use the root folder of your datapack (where the `data` folder and the `pack.mcmeta` file are) as a root folder of your workspace, so that DHP can provide you with the best functionalities.

Moreover, DHP fully supports VSCode's [multi-root workspace feature](https://code.visualstudio.com/docs/editor/multi-root-workspaces). Every root which contains a `data` folder and `pack.mcmeta` file will be considered as a datapack and will be used for computing completions. Other root folders will not be affected by DHP.

You can access any content of any root as long as they are in the same workspace. The order of the roots in your workspace will affect the priority of these datapacks in DHP. The root at the beginning will be loaded at first, and the root at the end will be loaded at last, which means that the **earlier** the root is, the **lower** priority in DHP it has. This is exactly how Minecraft loads datapacks and decide which one overrides another one if a file has the same namespaced ID and is in the same category. For example, if your multi-root workspace looks like this:

```
─── (Root) Datapack A
   ├── data
   |   └── spgoding
   |       └── functions
   |           └── foo.mcfunction
   └── pack.mcmeta
─── (Root) Datapack B
   ├── data
   |   └── spgoding
   |       └── functions
   |           └── foo.mcfunction
   └── pack.mcmeta
```

And then you use `F2` in a mcfunction file to renamed the mcfunction `spgoding:foo` to `wtf:foo`, only the file in Datapack B (`Datapack B/data/spgoding/functions/foo.mcfunction`) will be moved to `Datapack B/data/wtf/functions/foo.mcfunction`, even if there's a function with the same namespaced ID in Datapack A (`Datapack A/data/spgoding/functions/foo.mcfunction`).

If you try to execute these commands in Minecraft, you can also noticed that the function in Datapack B is executed.
```mcfunction
datapack enable "file/Datapack A" first
datapack enable "file/Datapack B" last
function spgoding:foo
```

By acting like this, DHP ensures that the order it handling datapacks is consistent with Minecraft.

**Note**: you can drag and put the root folders in VSCode to sort them, and DHP will update the priority of them in DHP accordingly, which is really handy.

## Multiple Language Support

DHP supports multiple languages. Currently these languages are available:

| Language             | VSCode Language ID |
| -------------------- | ------------------ |
| German               | `de`               |
| English              | `en`               |
| French               | `fr`               |
| Italian              | `it`               |
| Japanese             | `ja`               |
| Chinese (Simplified) | `zh-cn`            |

If you'd like to help us translate this project into other languages, it would be really appreciated! See [CONTRIBUTING.md](https://github.com/SPGoding/datapack-language-server/blob/master/CONTRIBUTING.md) for more information!

## Semantic Highlighting

> Wiki: https://github.com/SPGoding/datapack-language-server/wiki/Semantic-Coloring

All command arguments can be colored semantically. We also encourage you to install
[Arcensoth](https://github.com/Arcensotj)'s [language-mcfunction extension](https://github.com/Arcensoth/language-mcfunction)
to get instant coloring feedback.

![semantic-coloring](https://raw.githubusercontent.com/SPGoding/vscode-datapack-helper-plus/master/img/semantic-coloring.png)

## Signature Information

You can get hints about the arguments of commands while typing. Signature information will automatically show if
you commit a space character.

Alternatively you can use Ctrl + Shift + Space (or other configured hotkey) to show signature information manually.

![signature-help](https://raw.githubusercontent.com/SPGoding/vscode-datapack-helper-plus/master/img/signature-help.gif)

## Completions

The extension can compute completions as you typing commands. Completions will automatically show if
you commit one of these characters: `[' ', ',', '{', '[', '=', ':', '/', '!', "'", '"', '.', '@']`.

Alternatively you can use Ctrl + Space (or other configured hotkey) to show completions manually.

DHP can provide completions for simple commands:
![simple-completions](https://raw.githubusercontent.com/SPGoding/vscode-datapack-helper-plus/master/img/simple-completions.gif)

For more complex NBT tags, with help from [mc-nbtdoc](https://github.com/Yurihaia/mc-nbtdoc) made by [Yurihaia](https://github.com/Yurihaia):
![nbt-tag-completions](https://raw.githubusercontent.com/SPGoding/vscode-datapack-helper-plus/master/img/nbt-tag-completions.gif)

And also NBT paths:
![nbt-path-completions](https://raw.githubusercontent.com/SPGoding/vscode-datapack-helper-plus/master/img/nbt-path-completions.gif)

And also text components:
![text-component-completions](https://raw.githubusercontent.com/SPGoding/vscode-datapack-helper-plus/master/img/text-component-completions.gif)

And also NESTING THEM TOGETHER:
![ohhhhh-completions](https://raw.githubusercontent.com/SPGoding/vscode-datapack-helper-plus/master/img/ohhhhh-completions.gif)

## Code Snippets

DHP provides some helpful code snippets. See [VSCode's official docs](https://code.visualstudio.com/docs/editor/userdefinedsnippets) to learn more about code snippets. Both DHP and VSCode allow you to custom your code snippets, and they use exactly the same syntax because DHP is based on VSCode. For mcfunction files, code snippets added by DHP will be shown in the completions list only when the cursor is at the beginning of a command, however snippets added by VSCode's `Code/User/snippets/mcfunction.json` file will be shown everywhere in the file. If you want to custom your code snippets via VSCode, see [their official docs](https://code.visualstudio.com/docs/editor/userdefinedsnippets). If you want to custom snippets via DHP, see the [Configuration Settings section](#Configuration%20Settings).

![code-snippets](https://raw.githubusercontent.com/SPGoding/vscode-datapack-helper-plus/master/img/code-snippets.gif)

## Declaration Comments

> Wiki: https://github.com/SPGoding/datapack-language-server/wiki/Declare-Comment

You can use `#declare|#define <type: string> <id: string> [<description: string>]` to declare a string which will be used to compute completions, rename symbols, and find references/definitions by DHP. The game will treat declaration comments as normal comments and simply ignore them.

![declaration-comments](https://raw.githubusercontent.com/SPGoding/vscode-datapack-helper-plus/master/img/declaration-comments.png)

## Alias Comments

> Wiki: https://github.com/SPGoding/datapack-language-server/wiki/Alias-Comment

You can use `#alias <type: string> <alias: string> <value: string>` to define a string that will be shown in the completion list for specific arguments,
which will insert a different value when selected.

![alias-comments](https://raw.githubusercontent.com/SPGoding/vscode-datapack-helper-plus/master/img/alias-comments.gif)

## Diagnostics and Code Actions

> Wiki for code actions: https://github.com/SPGoding/datapack-language-server/wiki/Code-Actions

DHP provides real-time diagnostics about your commands. It can show syntax errors as Minecraft does, and even give your more detailed warnings.

Some diagnostics also have a corresponding quickfix action so that you can fix them easily.

![diagnostics](https://raw.githubusercontent.com/SPGoding/vscode-datapack-helper-plus/master/img/diagnostics.gif)

## Formatting and Linting

> Wiki: https://github.com/SPGoding/datapack-language-server/wiki/Lint-Rules

You can format the current function by pressing Shift + Alt + F or other configured hotkey.

Also there are several linting rules you can set in the configuration settings. DHP can report errors for commands that don't follow your preference.

![formatting](https://raw.githubusercontent.com/SPGoding/vscode-datapack-helper-plus/master/img/formatting.gif)

## Folding Ranges

You can use comments (`#region` and `#endregion`) to create folding ranges, which makes the structure of mcfunction file much clearer.

```mcfunction
#region This is a block of commands
execute if score @s test matches 1 run say 1
execute if score @s test matches 2 run say 2
execute if score @s test matches 3 run say 3
execute if score @s test matches 4 run say 4
execute if score @s test matches 5 run say 5
#endregion
```

Alternatively, you can use different amount of hash symbols followed by at least one white space to create different levels of folding ranges:

```mcfunction
#region This is a block of commands
# One
## One - 1
execute if score @s foo matches 1 run say 1
execute if score @s foo matches 2 run say 2
## One - 2
execute if score @s bar matches 1 run say 1
execute if score @s bar matches 2 run say 2
# Two
execute if score @p test matches 1 run say 1
#endregion
```

![folding-ranges](https://raw.githubusercontent.com/SPGoding/vscode-datapack-helper-plus/master/img/folding-ranges.gif)

## Call Hierarchy

_This feature is finished by using the proposed API, which is only accessible in development environment._

Call hierarchies are a great way to navigate through functions. You can get all the callers/callees of a function, function tag, or an advancement reward by using this feature. The default hotkey of this feature is `Shift + Alt + H`.

![call-hierarchy](https://raw.githubusercontent.com/SPGoding/vscode-datapack-helper-plus/master/img/call-hierarchy.gif)

## Color Information

DHP will display colors for `dust` particles and `color` tags in NBTs. You can change the color by hovering your cursor on it.

![color-information](https://raw.githubusercontent.com/SPGoding/vscode-datapack-helper-plus/master/img/color-information.gif)

## Hover Information

This is WIP.

DHP shows user-defined documentation when you hover on specific arguments, e.g. on function namespaced IDs, tags, teams, etc.

<!-- ![hover-on-function]() -->

## Resolving Namespaced IDs

You can navigate to advancements, loot tables, functions, predicates, and all kinds of tags by Ctrl-clicking on their namespaced IDs.

![document-link](https://raw.githubusercontent.com/SPGoding/vscode-datapack-helper-plus/master/img/document-link.gif)

## Goto Definitions

You can goto the definitions of objectives, entities, tags, teams, bossbars, and data storages in the workspace by Ctrl-clicking on their names.

![goto-definition](https://raw.githubusercontent.com/SPGoding/vscode-datapack-helper-plus/master/img/goto-definition.gif)

## Finding References

You can find all the references of objectives, entities, tags, teams, bossbars, data storages, advancements, functions, loot tables, predicates, recipes, and all kinds of datapack tags in the workspace by pressing Shift + F12 or other configured key.

![peek-references](https://raw.githubusercontent.com/SPGoding/vscode-datapack-helper-plus/master/img/peek-references.gif)

## Renaming

You can rename entities, tags, teams, bossbars, data storages, advancements, functions, loot tables, predicates, recipes, and all kinds of datapack tags by pressing F2 or other configured key on their names.

All the references of the same symbol in the whole workspace will be renamed.

![rename-objective](https://raw.githubusercontent.com/SPGoding/vscode-datapack-helper-plus/master/img/rename-objective.gif)

Additionally, if you rename a namespaced ID with file definition (e.g. the ID for advancement, function, loot table, predicate, recipe or tag), the corresponding file in the workspace will be renamed/moved too.

![rename-function](https://raw.githubusercontent.com/SPGoding/vscode-datapack-helper-plus/master/img/rename-function.gif)

*However*, renaming a file in a workspace manually will *not* update the namespaced IDs of it, and may cause problems with the cache.

## Configuration Settings

Press Ctrl + `,` (or other configured hotkey) to open the Settings page of VSCode, and search `datapack` to see all the configuration settings contributed by DHP. You can add your own code snippets, set the lint preferences and environment information to meet your needs. These config can be changed for the current user or the workspace. See [VSCode's official docs](https://code.visualstudio.com/docs/getstarted/settings) to learn more about configuring settings.

## Regenerating Cache

DHP uses cache to accelerate the process of renaming, finding references/definitions, document links, etc. However, the cache may become outdated because of various reasons, which could lead to strange behaviors. You can use the `Datapack: Regenerate Cache` command (ID: `datapack.regenerateCache`) to regenerate the cache manually.
