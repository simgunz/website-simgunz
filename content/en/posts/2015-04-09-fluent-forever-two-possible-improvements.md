---
title: 'Fluent Forever: two possible improvements'
author: Simone Gaiarin
date: 2015-04-09T06:33:15+00:00
images:
  - /images/languages-bubble.png
categories:
  - Anki
tags:
  - anki
  - fluent-forever
  - IPA
  - languages

---
In the last few months, I've spent some time in finding an effective and time efficient method for learning languages. After some research I've found a method called "Fluent Forever" formalised by Gabriel Wyner. This method is based on the spaced repetition software Anki.<!--more-->

### Anki

For the people who don't know Anki, it's a software which allows to memorise arbitrary facts in an extremely easy way by using flash cards. The software contains decks of cards, in which every card has a question on one side and the corresponding answer on the other side. The software asks the user to give the correct answer for a certain number of cards every day. If the user succeed, the software will schedule the next review of that card in an optimal period of time in the future which corresponds to the moment just before the user will forget that facts. In this way the facts are learned by reviewing each card the minimum number of times required to not forget them (as opposed to the cram method which is much less effective for learning on the long term). If the user gives the wrong answer, the card should be reviewed again and the scheduling information are reset.

Even people who thinks to have a bad memory can learn huge amount of information easily by using Anki. Moreover, it has mobile apps either for Android and iOS, so that it's possible to study everywhere making dead times productive. For more information about Anki visit the official site [http://ankisrs.net][1].

### Fluent Forever

I consider Fluent Forever a scientific method for learning languages. It's not a method that promises to let you learn a language easily and without effort, but I consider it the easiest way to learn a language, or at least a very useful support for learning a language, even though it requires a little bit of effort at the beginning. I'll explain why in a moment.  
To learn a new language in the best way from the beginning, the first thing to do is learning the sounds of it. Often a new language contains sounds not present in our native language, and because of this it's very important to learn the new sounds very soon, so that we'll be able to pronounce the words in the best way possible without sounding too "foreigners" to the native speakers. Indeed once we learnt a new word in the wrong way, it's hard to "rewire" our brain to learn the correct pronunciation. Moreover, learning the correct sound allows us to improve our listening skills.

An extremely useful aid to learn the pronunciations of the words is the International Phonetic Alphabet (IPA) which is a collection of standard symbols which map one-to-one to every possible sound a human can produce. Gabriel Wyner has created a series of videos where he goes through all the most useful symbols of the IPA. I've found the IPA an extremely useful tool. As an example of its potentiality, when we read a book on Google Play books or on Kindle we can lookup the definition of a word, and by reading the IPA transcription without hearing the pronunciation we  can have a pretty good idea of how that word sounds.

The first phase of the method, called pronunciation-trainer, is composed by one Anki deck used for learning spelling rules, i.e., how to map graphemes (letter or cluster of letters) to sounds, and a second deck used to train with minimal pairs, which are couple of words which differs on a single sound like _buy _[baɪ]_ _and _pie _[paɪ]. Minimal pairs allows our brain to learn the new sounds present in the target language and to distinguish them from sounds that sounds very similar.

The second phase of the method deals with learning the vocabulary of a language. Each flash card in this Anki deck shows a picture of a word on on side and on the other side shows the transcription of the word, the IPA transcription, and the audio pronunciation of it. To learn words in this way, it's important that each user builds the words deck by himself, because the act of choosing a particular picture or typing in the word is already part of the learning process. (There are some other matters I haven't discussed, like mnemonics, how to deal with abstract words and so on). I suggest to have a look at <http://fluent-forever.com> in order to obtain more information on the method and a lot of other useful resources.  
<a name="twoproblems"></a>

## Fluent-forever in real life: two problems

<a name="twoproblems"></a>  
So let's get to the point of this post. I'm currently trying the Fluent Forever method to learn Danish, but I've found some hiccups right at the beginning.

**Problem one** Danish uses it's own variation of the IPA in order to avoid polluting the IPA transcription with tons of diacritics, so it uses some symbols to represent different sounds respect what they usually represent in the standard IPA. As an example, the symbol _ɛ_ which in the standard IPA represents an _open-mid front unrounded_ vowel, in Danish represents a _closed-mid front unrounded_ vowel, and the _open-mid front unrounded_ vowel is represented by <!--anki-->

_æ_. So if someone has learned the IPA for the English language may be very confused when first try to read the IPA for the Danish. Because of this, it's necessary to learn a specific IPA for Danish (as a side note, it's interesting to know that Danes developed a custom phonetic alphabet called _dania_ to make it easier the transcription of the rather complex Danish sound system). I've noticed that this is also true for other languages even though in a weaker way. In particular it's true that in most of the freely available resources on line most of the diacritics are often omitted. Even tough this is not a huge problem for most of the languages, I think that the Fluent Forever method would be more complete if before dealing with the spelling rules and minimal pairs it let the user study all the IPA symbols for the specific language. Most important, an IPA specific deck can include the audio of the sounds pronounced by native speakers, which certainly  sounds more naturally than the audio present on Wikipedia. Most of the users can probably survive without using this extra deck, and probably they can also learn the language without learning the IPA at all, but for those like me, which think that the IPA is an extraordinary useful tool and want to learn it in the best way possible for each language I've started creating a set of Anki decks for language specific IPA. I'll present my first results in the next article.

**Problem two  **The second problem of the method appears when we start to build the vocabulary deck, and it's pretty easy: it takes a huge amount of time to build it. In fact for every card we need to:

  * Search an image that represents the word on google images or similar services and drag it into the Anki card editor. This operation is a pretty fast, even tough it requires some clicks to reach the source of the image (even with the basic version of google images).
  * Search the word pronunciation on forvo.com, download it and load it in the Anki card editor. This operation is longer, because we need to navigate some pages in forvo.com, download the audio and drag it into the Anki editor.
  * Search the word IPA transcription in some on line dictionaries or in wikitionary.org and copy it into the Anki card editor. Also this operation requires some time.

All these operation takes time. I've estimated that the generation of a single card takes about 45 seconds when we hit the correct meaning of the word at the first trial. That means that for building a deck of 3000 words it takes about 37.5 hours, which is quite a lot. Considering that sometimes we need to spend extra time to find the images for the correct meaning of the word, this amount of time is probably bigger. Because of this, I've decided to develop an Anki add-on to integrate all these operations into the Anki editor in order to speed up the whole process. In particular in this way it's possible to eliminate all the time-consuming operations which are not useful for the learning process (like surfing between web pages and downloading stuff) so that we can concentrate only on the meaning of the word we are processing. I'll present some mock-ups of the add-on in a next article.

