---
title: KTouch lessons generator
author: Simone Gaiarin
date: 2015-07-12T14:30:09+00:00
images:
  - /images/feature/ktouch.jpg
tags:
  - kde
---
Recently I've decided to learn how to type with 10 fingers, so I've started using KTouch for learning it. I chose the predefined lesson for the Italian keyboard, and I really enjoyed it. On average, it took about one hour for passing one lesson (two new letters), which I think is a pretty reasonable time.<!--more-->

The problem I've found is that that lessons don't include Italian accented letters, nor numbers or symbols. Moreover the lessons regarding foreign letters `j k x y w` don't include any word containing these letters, probably because of a poor word picking algorithm.

I've searched a little around for the script for generating the lessons but I couldn't find it (I didn't search a lot actually). So I decided to write a python script to generate arbitrary lessons for any language starting from a dictionary and a file containing the list of letters we want to learn every lesson.

The script is still a little bit rough, and it doesn't generate a xml file compatible with KTouch, so that we need to copy and paste each lesson, but for the rest is pretty nice and it just work. I'll try to improve it as soon I'll have time.

### Features:

  * Generate lessons using words from a dictionary file
  * Generate lessons by combing letters randomly
  * Generate lessons with symbols and number
  * Support specifier for symbols positioning (e.g. we can specify that a comma must always be placed right to a word and never left to it)
  * Many other fine tuning parameters can be used to customize the lesson (symbol density, include/not include previous learned symbols/number, etc.)

### Problems:

  * The auto-generated lessons favour longer combination of letters
  * The order of the new letters is important and different combinations need to be tried manually to optimize the result. E.g. in Italian the letter `q` is always followed by the letter `u`, so if we try to learn the letter `q` before `u` the resulting lesson won't contain any word including `q`. Another example is when in a lesson we try to learn an Italian common letter like `z` together with an uncommon one like `y`; likely the resulting lesson won't contain any word including `y`.

The script can be found from [github](https://github.com/simgunz/ktouch-lesson-generator) together with an example dictionary and lessons list.
