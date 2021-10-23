+++
title = "Redshift plasmoid"
description = "KDE plasma widget to interact with redshift"
date = "2021-08-22"
author = "Simone Gaiarin"
postDate = false
readingTime = true
+++

Redshift Plasmoid provides a graphical configuration interface to redshift, and it allows the user to start/stop the redshift daemon. The program automatically retrieves the user coordinates from the KDE settings, permitting redshift to determine the current position of the sun, needed to perform the color regulation.

The program extends the original redshift with the following extra features:

  * **KDE activities support** A different behavior of redshift can be set for different activities. The possible behaviors are: always on, always off or auto. As an example, redshift can be automatically turned off when the user switches to the movie activity, so that the movie can be seen without altering the original colors.
  * **Manual color adjustment** Sometimes the color temperature set does not match the current environmental light. It can happen when we are in a different location, where there are different types of light, or when there is a gloomy day and the sunlight is not very bright. In these cases it is possible to manually adjust the color temperature by spinning the mouse wheel over the widget. The program will then switch to manual mode and won't adjust the temperature automatically, but will keep the current. To exit manual mode you just click on the widget.

## Redshift

Redshift adjusts the color temperature of the screen trying to match that of the ambient light. During the day, the sun enlightens the surroundings with a bright light which corresponds to a color temperature of 5500K-6500K. At night the light is provided by the lamps in the room. Depending on the type of the lamp, the color temperature can be warm, as in the case of bulb light, or brighter as for fluorescent light, so the color temperature should be set correspondingly. Matching the color temperature of the screen to that of the ambient light help your eyes don't get strained, and try to minimize the negative effects of exposure to bright light during the night.

## Scientific motivation

Many researches have been able to prove that the exposure to the bright light emitted by electronic devices like computer monitors, smartphones and tablets during the night, can lead to sleep disorders. The blue undertone, even if not directly perceived, is associated by the human body with daytime. This abnormal exposure to blue light during the night can make us more alert and improve our response times. It also has been shown that it suppresses melatonin, a hormone that regulates the sleep-wake cycle.

# Downloads

[GitHub](https://github.com/simgunz/redshift-plasmoid)

# Screenshots

# References

The Redshift daemon is developed by Jon Lund Steffensen (http://jonls.dk/redshift/)
