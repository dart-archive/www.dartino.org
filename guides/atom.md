---
title: Dartino Atom IDE
layout: page
---

# Dartino Atom IDE

The Dartino SDK provides a fully featured IDE (Integrated Development
Environment)! This complements the [`dartino` command line tool](/guides/tool/)
by enabling you to edit your code, run, flash, and debug from a single,
integrated graphical environment. The Dartino IDE functionality is built as a
set of free and open-source plug-ins to the popular open-source Atom IDE
framework enabling you to further customize the IDE experience if desired.

### Installation and setup

Please follow [these instructions](https://atom.io/packages/dartino).

## Key features

### Code navigation

The left-hand pane is used to navigate between files. To add content to the
pane, use `File->Add Project Folder...` and select the root directory where
your dartino program is located. To view all Dartino samples, select
`Packages->Dartino->Open Samples...`.

You can also navigate the source code based on a rich code analysis. To navigate
to the declaration of a used symbol, right-click and select 'Go to declaration'.

### Code completion

As you type, your code will be analyzed and the IDE will offer completions:
![Code completion screenshot](/images/atom-code-completion.png)

These completions are based on a full type analysis on your code, and thus help
you write correct programs faster!

### Live error checking

As you type, your code is analyzed for errors and warnings. These are indicated
with yellow (for warnings) and red (for errors) underlines. If you place the
cursor over the error, a tooltip will display details:
![Warning screenshot](/images/atom-warning.png)

All errors and warnings are summarized in the bottom issue pane:
![Issues pane screenshot](/images/atom-issues.png)

### Running Dartino programs

You can build and flash from the IDE using these steps:

1. Locate the toolbar at the top of the IDE

1. In the drop-down box select the program that you want to run

1. Press the Play button. This will build and flash the program.

## Keyboard shortcuts

*Note*: On Linux, replace `cmd` below with `ctrl`.

### General

| Shortcut                  | Action                                      |
| :-------------------------|:--------------------------------------------|
| `cmd-n`                   | Create a new file                           |
| `shift-cmd-o`             | Open a project folder                       |
| `cmd-tab`                 | Toggle to next open file                    |
| `cmd-f`                   | Find in current file                        |
| `cmd-shift-f`             | Find in current project                     |


### Dartino

| Shortcut                  | Action                                      |
| :-------------------------|:--------------------------------------------|
| `cmd-r`                   | Run current program                         |
| `alt-cmd-b`               | Automatically format current file           |
| `alt-cmd-arrow down`      | Go to: declaration                          |
| `alt-cmd-arrow up`        | Go to: previous location                    |
| `alt-shift-r`             | Refactoring: rename                         |
| `alt-shift-i`             | Refactoring: inline local                   |
