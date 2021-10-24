+++
title = "Timekpr KDE"
description = ""
date = "2021-08-22"
author = "Simone Gaiarin"
postDate = false
readingTime = true
images = [ "images/feature/timekpr.jpg" ]

+++

Timekpr allows the user to limit the computer daily usage based on allowed login time window and on the login duration period. The application can be used as a parental time control to limit child access time (or as a self limiting program to spend less time on Internet and enjoy the open air). Timekpr comes with a configuration module in the KDE settings page, where it is possible to set the allowed time window and the session duration limit either on a daily basis or globally. Next to the configuration module there is a console where it is possible to grant extra time to a user, clear all the limitations, lock the account, etc.

### Technical details

The project was born as a KDE interface to the original Timekpr, but it has evolved over the time. The backend have been rewritten, adding among other things the dbus support so that it can communicate with the kcm module and with the plasma dataengine. A plasmoid will provide the notification system to alert the user when the session time is expiring.

The project is still in an alpha stage, some pieces are missing (e.g. lock account, notification system, gtk interface (?)), but it's already usable.

 **Timekpr sudo inhibitor** Sometimes timekpr can be useful to limit ourselves instead of our child. If you are the administrator of the system you can use timekpr to limit yourself, but of course you can stop the daemon and avoid all the limitations. A way to limit an administrator user is needed. Here technology can't do much but we can use psychology. Example case: It's night and it's time to go to sleep, but you are still on facebook snooping on friends profile or implementing a new exciting feature for timekpr (I fall in the second case) and you think "In 30 minutes I swear I'll go to sleep". After 30 minutes you think, "Ok, in 10 minutes I'll go to sleep" and so on for hours.

The timekpr sudo inhibitor function can come in our help in this case. Enabling this feature for a sudo user Timekpr will revoke the administrator privileges 30 minutes before the logout time. At this point the user is in the phase where he really thinks he will shutdown the computer in 30 minutes and it's not motivated to stop the daemon, when after 30 minutes timekpr is about to logout the user and he is in the phase “Ten minutes more” he can't stop the daemon because he lost all the administrator privileges.

To enable this function for a user, the sudo user privilege should be written in a file called /etc/sudoers.d/tkpr-[USER] rather than in the sudoers file.

## Downloads

[GitHub](https://github.com/simgunz/timekpr)

## Screenshots

![Redshift screenshot 1](images/timekpr-scr-1.jpg#center)
![Redshift screenshot 1](images/timekpr-scr-1.jpg#center)

## References

Timekpr was originally developed by Even Nedberg (https://launchpad.net/timekpr)
