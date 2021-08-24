---
title: KTouch lessons generator
author: Simone Gaiarin
date: 2015-07-12T14:30:09+00:00
images:
  - /images/ktouch.png
categories:
  - KDE
tags:
  - kde

---
Recently I&#8217;ve decided to learn how to type with 10 fingers, so I&#8217;ve started using KTouch for learning it. I chose the predefined lesson for the Italian keyboard, and I really enjoyed it. On average, it took about one hour for passing one lesson (two new letters), which I think is a pretty reasonable time.<!--more-->

The problem I&#8217;ve found is that that lessons don&#8217;t include Italian accented letters, nor numbers or symbols. Moreover the lessons regarding foreign letters (jkxyw) don&#8217;t include any word containing these letters, probably because of a poor word picking algorithm.

I&#8217;ve searched a little around for the script for generating the lessons but I couldn&#8217;t find it (I didn&#8217;t search a lot actually). So I decided to write a python script to generate arbitrary lessons for any language starting from a dictionary and a file containing the list of letters we want to learn every lesson.

The script is still a little bit rough, and it doesn&#8217;t generate a xml file compatible with KTouch, so that we need to copy and paste each lesson, but for the rest is pretty nice and it just work. I&#8217;ll try to improve it as soon I&#8217;ll have time.

### Features:

  * Generate lessons using words from a dictionary file
  * Generate lessons by combing letters randomly
  * Generate lessons with symbols and number
  * Support specifier for symbols positioning (e.g. we can specify that a comma must always be placed right to a word and never left to it)
  * Many other fine tuning parameters can be used to customize the lesson (symbol density, include/not include previous learned symbols/number, etc.)

### Problems:

  * The auto-generated lessons favour longer combination of letters
  * The order of the new letters is important and different combinations need to be tried manually to optimize the result. E.g. in Italian the letter &#8216;q&#8217; is always followed by the letter &#8216;u&#8217;, so if we try to learn the letter &#8216;q&#8217; before &#8216;u&#8217; the resulting lesson won&#8217;t contain any word including &#8216;q&#8217;. Another example is when in a lesson we try to learn an Italian common letter like &#8216;z&#8217; together with an uncommon one like &#8216;y&#8217;; likely the resulting lesson won&#8217;t contain any word including &#8216;y&#8217;.

The script can be found on my github page, <a href="https://github.com/simgunz/ktouch-lesson-generator" target="_blank">here</a>, together with an example dictionary and lessons list.

&nbsp;
