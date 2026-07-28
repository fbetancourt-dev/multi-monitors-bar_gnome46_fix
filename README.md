# Multi-Monitors-Bar Extension (GNOME 46 Fix for Disposed Object Crash)

## Overview
This repository contains the fixed source code for the GNOME Shell extension `multi-monitors-bar@frederykabryan`, patched to resolve GJS `Object has been already disposed` exceptions on GNOME 46 / Ubuntu 24.04 LTS.

## Root Cause
When secondary displays or status indicators are disconnected or redrawn, GNOME Shell destroys underlying C/GTK objects before GJS extension signal handlers are cleaned up. Calling `.disconnect()` or `.get_first_child()` on destroyed objects threw uncaught exceptions:

```text
Object .Gjs_ubuntu-appindicators_ubuntu_com_indicatorStatusIcon_IndicatorStatusIcon has been already disposed — impossible to access it.
#0 mirroredIndicatorButton.js:3356
```

## Implemented Fix
In `mirroredIndicatorButton.js` (lines 3345-3360), signal disconnection and child indicator inspection during `_cleanup()` are wrapped in defensive `try { ... } catch (e) {}` blocks. Disposed C objects are safely ignored without crashing GNOME Shell.

## Installation
To install or update the extension locally:

```bash
mkdir -p ~/.local/share/gnome-shell/extensions/multi-monitors-bar@frederykabryan
cp -r * ~/.local/share/gnome-shell/extensions/multi-monitors-bar@frederykabryan/
gnome-extensions disable multi-monitors-bar@frederykabryan
gnome-extensions enable multi-monitors-bar@frederykabryan
```
