---
title: Redshift plasmoid 0.5 released
author: Simone Gaiarin
date: 2012-12-09T09:26:09+00:00
images:
  - /images/redshift_large.png
categories:
  - Projects
tags:
  - kde
  - redshift

---
Finally I found some time to update redshift plasmoid.

The major change in the new version is the possibility to set the temperature color manually, a.k.a manual mode. To do so you just need to scroll the mouse wheel over the widget and immediately it switch to manual mode, starting to set the color temperature you desire.  <!--more--> When it is in manual mode, redshift won't keep set the color temperature but just hold the current one. To switch back to auto mode it's enough to click on the widget.

Moreover, redshift has become translatable so don't hesitate to translate it. I need your help to do this! To translate redshift you can use the kde translation system lokalize. You just need to setup a new project, open the .pot file you find in the redshift source folder, save it as xx.po, where xx is your language code, and translate every item through lokalize.

I hope you'll enjoy this new version.

Changelog:

  * Added manual mode
  * Improved some labels and tooltips
  * Added translation support
  * Added Italian translation
