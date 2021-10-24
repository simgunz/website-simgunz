+++
title = "Activity Manager Wheeled"
description = ""
date = "2021-08-22"
author = "Simone Gaiarin"
postDate = false
readingTime = true

+++

Activity Manager wheeled is plasmoid that allows the user to manage the KDE  Plasma desktop activities effectively. It can start, stop, add, edit and remove activities and switch between them.

In the current version of Activity Manager I've only added the support to change activity by scrolling the mouse wheel over the widget. I've called this version "Activity Manager Wheeled" to distinguish it from the vanilla version, since the author didn't include my patch in his version.

I've tried to rewrite the application from scratch in QML to improve it and to add some missing features, like the ability to add a new activity from template, but the lack of public API on the class required to fully control the KDE activities has made impossible to complete the work.

## Downloads {#screenshots}

[GitHub](https://github.com/simgunz/activitymanager-wheeled)

## Screenshots

![Activity manager screenshot 1](images/activity-manager-scr-1.jpg#center)

## References

The original Activity Manager was developed by Abdurrahman Avci

KDE Apps: [Activity Manager](http://kde-apps.org/content/show.php/?content=136278)

KDE Apps: [Activity Manager Wheeled](http://kde-apps.org/content/show.php/Activity+Manager+Plasmoid+Wheeled?content)
