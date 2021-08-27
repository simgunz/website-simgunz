---
title: On the state of Activity Manager
author: Simone Gaiarin
date: 2014-10-03T08:06:58+00:00
images:
  - /images/activities_wide.png
categories:
  - KDE
  - Projects
tags:
  - kde

---
As is written in the project page of Activity Manager, "I'm currently rewriting it from scratch in QML to improve it and to add some missing features, like the ability to add a new activity from template". Actually, after trying different way to interact with the KDE activities I'm stuck.

<!--more-->Even though KDE allow to interact with the activities system in an easy way through the org.kde.activities dataengine or through dbus, it doesn't expose all the classes needed to write a full featured activity manager. In particular it is not possible to create new activity from template. The KActivityController class, that is the one that allow a full control over the activities, is not public.

Because of this, my work on the QML activity manager has stopped long time ago and the project can be considered dead. Moreover KDE 5 is already out and I hope there will be a new and better activity manager plasmoid.
