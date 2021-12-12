---
title: "Anki add-ons bulk update"
description: ""
date: 2021-12-11T14:42:32+01:00
featured: false
draft: false
comment: true
toc: true
reward: true
categories: Projects
tags: 
  - anki
images:
  - /images/feature/anki.jpg
---

In the last months I have updated all my anki addons to make them compatible with the newer versions of anki, here is a recap of the changes.

<!--more-->

## Fast cards reposition

- Thanks to the contribution of [ijgnd](https://github.com/ijgnd) this addon now works with anki 2.1.45+ and has configurable shortcuts.
- The add-on is now aware of the browser note mode and disables the menu actions when this mode is enabled.
- Completely refactored the code, which should make the addon more maintainable now.

## Minimize to tray

- Improved the behavior of the add-on when clicking on the tray icon
  - if any anki window is minimized, show all windows
  - if anki is minimized to the system tray, show all windows
  - if the anki window is not in focus, focus it
  - if the anki window is in focus, minimize to tray (this works only in linux and mac)
- Fixed the exception thrown when trying to focus a deleted widget
- Fixed a problem in Windows that was preventing anki to start if `hide_on_startup` is `true` and there is no internet connection

## Maximum image height and width in card editor

- Add-on adapted to work with anki 2.1.41+

