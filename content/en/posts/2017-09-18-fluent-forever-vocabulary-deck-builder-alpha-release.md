---
title: Fluent Forever vocabulary deck builder pre-alpha release
author: Simone Gaiarin
date: 2017-09-18T13:19:28+00:00
images:
  - images/ff-anki-alpha.png
categories:
  - Anki
  - Projects
tags:
  - anki
  - fluent-forever

---
After more than two years of silence, here I&#8217;m back again. Today I&#8217;ve pushed to github the fluent forever vocabulary deck builder for anki.<!--more-->

First I need to apologize for this long silent time, but my PhD and a lot of traveling kept me very busy so that all my projects stalled. Although, I still want to keep my projects alive and I hope to find soon more time to work on them.

I wanted to do some extra work on the fluent forever vocabulary deck builder plugin for anki before releasing it to the public, but given the fact that I don&#8217;t have so much time to give it the love it deserves, and the fact that many of you are waiting for this plugin, I decided to release it even if incomplete, so that who wants can start using it and contribute to it. As it is now, all the python dependencies need to be installed manually and it was never tested under windows, so you need a minimum of skills to make it work. Moreover, many features are incomplete or missing, and I&#8217;m aware of it. Finally, Anki 2.1 is coming soon and it will break completely the compatibility with the add-on. I started porting the add-on to Anki 2.1 but this is still a work in progress, and as for now the add-on doesn&#8217;t work with Anki 2.1.

Hopefully, I&#8217;ll go back to work on it soon, in order to polish the code and to make a basic working version.

In the meanwhile I hope someone is willing to contribute to it in any way, to push this project forward. Any type of feedback is welcomed.

## Current features

  * Gallery loading from Bing images (requires API key, ugly looking)
  * Automatic image rescaling (to minimize disk space and speed up loading in anki)
  * Download audio from Forvo (requires API key)
  * Filter noise and normalize volume form audio tracks
  * IPA download from Wiktionary [on a separate branch]
  * Preloading
  * Embed of Wikitionary and a custom website to manually lookup definitions (and other information) [on a separate branch]
  * Configuration dialog to setup API keys and select current language
  * Anki browser navigation buttons (Previous, Next)

An Anki template deck is provided together with the plugin to minimize the effort. In any case, the plugin works on any deck with the following fields: Picture, Pronunciation sound, IPA transcription. For now, all the 3 fields need to be present.

## Code

