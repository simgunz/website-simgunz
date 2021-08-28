---
title: Fluent Forever vocabulary deck builder progress update
author: Simone Gaiarin
date: 2015-07-15T21:55:17+00:00
images:
  - /images/ff-anki.png
categories:
  - Projects
tags:
  - anki
  - fluent-forever
---
In the last months I've been working on the implementation of the Fluent Forever anki add-on, which I first introduced as a mock up in a [previous post][1]. The work has progressed, and now I have a working version with some features, here listed.<!--more-->

### Current features

  * Gallery loading from Bing images
  * Download audio from Forvo (see note below)
  * Filter and normalize audio (only linux, easily portable to Win, Mac)
  * Image and sound preloading
  * Embed of Wikitionary and a custom site to retrieve definition (and other information)
  * Browser navigation buttons

### What's missing

  * Load galleries regarding multiple queries
  * Allow to use a custom query for image search
  * IPA download
  * Part of Speech (PoS) download
  * Option to tune the noise suppression level (to avoid audio artifacts)
  * Many others features

## Video demo

Here a demonstration of the plugin in action, as is, just to give an idea of the state of the development (even tough most of the stuff that have been done are under the hood).

{{< youtube _BLhlh0zb4A >}}

## Problems to be addressed

Trying to use the script to build an Anki deck of words I've faced some issues I didn't think about before. In particular I've created a deck starting from a [frequency list][2] built automatically by analyzing the  subtitles present on opensubtitles.org, and the problems I've found are:

  * Some words have multiple uncorrelated meanings, so that it's necessary to duplicate the card in order to split the multiple meanings. This is extremely time consuming, and needs to be automatized.
  * Many words with high frequency are abstract words or derived words such plurals, conjugated verbs, etc. which need to be suspended in a first moment, when we are learning the basic vocabulary of easy and tangible words. I'm thinking of implementing buttons to suspend and skip the current words and possibly tag them as "plurals", "abstract", etc. so that can be easily processed in a second moment.

Another big problem I've found regards Forvo API license. In particular the license states that  _"It is not allowed to cache audio pronunciations.",_ which implies that it's not possible for an application to save the content downloaded via the API. On the other hand the site content is licensed as CreativeCommon and on the API non-profit plan (which now is not free any more) they state that it's intended _"For academic and individual use with Anki, GoldenDict etc."_ so I can't understand why there is that clause in the API license. I've written them but I'm still waiting for a response. In any case, the very good Anki plugin "Audio downloader" can be adapted to retrieve word pronunciations from many other sites.

Finally I want to specify that, currently I'm manly focusing on European languages, so I'm not dealing with languages which employs symbols like Chinese, which require special care. I'll address these kind of languages as soon as I have all the basic features implemented.

 [1]: /posts/2015-04-25-fluent-forever-vocabulary-deck-builder-for-anki
 [2]: https://invokeit.wordpress.com/frequency-word-lists
