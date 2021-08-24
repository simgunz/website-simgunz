---
title: Fluent Forever vocabulary deck builder progress update
author: Simone Gaiarin
date: 2015-07-15T21:55:17+00:00
images:
  - /images/ff-anki.png
categories:
  - Anki
  - Projects
tags:
  - anki
  - fluent-forever

---
In the last months I&#8217;ve been working on the implementation of the Fluent Forever anki add-on, which I first introduced as a mock up in a [previous post][1]. The work has progressed, and now I have a working version with some features, here listed.<!--more-->

### Current features

  * Gallery loading from Bing images
  * Download audio from Forvo (see note below)
  * Filter and normalize audio (only linux, easily portable to Win, Mac)
  * Image and sound preloading
  * Embed of Wikitionary and a custom site to retrieve definition (and other information)
  * Browser navigation buttons

### What&#8217;s missing

  * Load galleries regarding multiple queries
  * Allow to use a custom query for image search
  * IPA download
  * Part of Speech (PoS) download
  * Option to tune the noise suppression level (to avoid audio artifacts)
  * Many others features

## Video demo

Here a demonstration of the plugin in action, as is, just to give an idea of the state of the development (even tough most of the stuff that have been done are under the hood).



## Problems to be addressed

Trying to use the script to build an Anki deck of words I&#8217;ve faced some issues I didn&#8217;t think about before. In particular I&#8217;ve created a deck starting from a [frequency list][2] built automatically by analyzing the  subtitles present on opensubtitles.org, and the problems I&#8217;ve found are:

  * Some words have multiple uncorrelated meanings, so that it&#8217;s necessary to duplicate the card in order to split the multiple meanings. This is extremely time consuming, and needs to be automatized.
  * Many words with high frequency are abstract words or derived words such plurals, conjugated verbs, etc. which need to be suspended in a first moment, when we are learning the basic vocabulary of easy and tangible words. I&#8217;m thinking of implementing buttons to suspend and skip the current words and possibly tag them as &#8220;plurals&#8221;, &#8220;abstract&#8221;, etc. so that can be easily processed in a second moment.

Another big problem I&#8217;ve found regards Forvo API license. In particular the license states that  _&#8220;It is not allowed to cache audio pronunciations.&#8221;,_ which implies that it&#8217;s not possible for an application to save the content downloaded via the API. On the other hand the site content is licensed as CreativeCommon and on the API non-profit plan (which now is not free any more) they state that it&#8217;s intended _&#8220;For academic and individual use with Anki, GoldenDict etc.&#8221;_ so I can&#8217;t understand why there is that clause in the API license. I&#8217;ve written them but I&#8217;m still waiting for a response. In any case, the very good Anki plugin &#8220;Audio downloader&#8221; can be adapted to retrieve word pronunciations from many other sites.

Finally I want to specify that, currently I&#8217;m manly focusing on European languages, so I&#8217;m not dealing with languages which employs symbols like Chinese, which require special care. I&#8217;ll address these kind of languages as soon as I have all the basic features implemented.

 [1]: http://simgunz.org/fluent-forever-vocabulary-deck-builder-for-anki/
