---
title: Fluent Forever vocabulary deck builder for Anki
author: Simone Gaiarin
date: 2015-04-25T10:30:43+00:00
images:
  - /images/ff-anki.png
categories:
  - Anki
tags:
  - anki
  - fluent-forever

---
In a [previous post][1] I've discussed about two problems of the Fluent Forever method. Here I present the idea I had to try to solve the second one, i.e., how to build vocabulary decks in a faster way.

The basic idea is to create an Anki add-on that automatically searches and downloads all the media we need to create our cards.<!--more-->In particular it should be able to:

  1. Using an image service like Google images or Bing images, load an image gallery directly into the Anki card editor so that the user can choose the image that better describe the current word just clicking on it.
  2. Using a word pronunciation service (e.g. <a title="Forvo" href="http://forvo.com" target="_blank">forvo.com</a>), load a "gallery" of the available pronunciations of the current word in the target language.
  3. Using on line dictionaries, load a "gallery" of the possible IPA transcriptions. Multiple transcriptions may be available for various reasons. As an example different services may have slightly different transcriptions due to different conventions, like using a more strict IPA transcription which uses more diacritics or they can provide General American and Received Pronunciation transcriptions in the case of English.
  4. Preload all the media for the next words in order to minimize the creation time of each card.

In the following video you can see a semi-working mock up of this add-on with explanations, and in the [appendix][2] of this post there are two other videos which compare the current card creation workflow to the workflow using the add on.



&nbsp;

## Issues and possible solutions

Unfortunately not all the cards are easy to build like the ones shown in the video above, so that some advanced things must be taken into account. Here I discuss some of them.

### Image gallery

#### Problems

  1. Some words may have multiple meanings.
  2. Some words may have different meanings in different languages (e,g. killing (da) = kitten (en)) which may lead to wrong results when searching for images.

#### Possible solutions

  1. Use smart search strategies, e.g., search results only in the target language, preload multiple search results using the combination of the original word and its translation.
  2. Allow the user to perform a custom search or add an extra search terms to the word to improve the results.

### Pronunciation gallery

#### Problems

  1. Many audio files have very bad quality and a lot of noise.
  2. Some pronunciations may be missing.

#### Possible solutions

  1. Automatically filter noise (I'm working on some code which do this, and the results are not bad).
  2. Automatically request a pronunciation for the given word or open a page to do it in services like [forvo.com][3] or <a title="Rhinospike" href="http://rhinospike.com" target="_blank">rhinospike.com</a>

### IPA gallery

#### Problems

  1. The user may not be able to evaluate which transcription is the best.
  2. The quality of the transcriptions are not optimal.
  3. The transcription is missing.

#### Possible solutions

  1. Use multiple services.
  2. Let the users rate the transcriptions and share the rates among the users.

I want to expand the last point. An useful tool that currently is missing, is a service that collects and let access in a standard way the IPA transcriptions of all the words. A kind of Forvo for the IPA. Indeed, even tough it's possible to find IPA transcriptions on line, the transcriptions are not available for all the words of a language, or only the phonemic transcription is available, or the transcription is given in non standard IPA. As an example Google "define", gives the phonemic transcription in non-IPA transcription, Google Books only provides the IPA phonetic transcription for the basic form of a verb so that it's not possible to obtain the IPA transcription for words like "was" because you will get the definition and transcription of  "be", but "was" is a completely different word and it's pronounced in a different way. The only tool that provides IPA for most of the words is Wikitionary, but it has no sort of API to access the different fields.

I've recently discovered that Forvo provides IPA transcriptions for some words, but only editors can add more transcriptions, which of course makes it very difficult to provide IPA transcription for all the words of all the languages. I've suggested the guys of Forvo to allow users add more IPA transcriptions, and create a rating system (as the one already present for the audio pronunciations) in such a way to have a quality indicator of the different transcriptions. At that point Forvo would be a complete tool for language learning.

If you would like to see this feature implemented in Forvo ask the Forvo developers to implement it! If they see there is a wide interest in this feature they may be more motivated in add it.

### Other issues

One user pointed out that for languages like Chinese and Japanese there are some other problems, which need to be addressed as well.

_"I'm trying Mandarin Chinese via FF, and there's a bit more to do – compounded by having to get the characters with correct stroke order (needing more mnemonics to help the brain cells), plus the fact that G-Images for Chinese has a strong tendency to return images of women wearing few clothes, regardless of the input!!"_

## Other possible features

  * **Mnemonic manager.** Create a collection of mnemonic images and allow adding them easily to the different cards.
  * **Word definition.** Show the definition of the word in the target language to better understand its meaning and help in the card build operation.

## Conclusions

With this post I've provided an overview of my ideas on the add-on implementation. Now I want to hear what you think about it, if you consider it useful, which features can be added, what problems should be addressed and so on, so that we can make a further step in the simplification of the hard task of learning a new language.

## 

## Appendix: Card creation workflows comparison {#appendix}

&nbsp;



&nbsp;



 [1]: http://simgunz.org/fluent-forever-two-possible-improvements/#twoproblems "Fluent Forever: two possible improvements"
 [2]: #appendix
