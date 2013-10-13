---
layout: post
title: "Setting up the Android NDK"
description: "How to set up the Android NDK"
category: "Android-NDK"
tags: [Android NDK, Linux]
comments: false
---
{% include JB/setup %}
##Setting up the Android NDK
---
Currently I wanted to make some XPath benchmark tests on my Samsung Galaxy Tab2 10.1 (p5110).<br>
Therefore I needed xmllint to run on Android. Problem here is that xmllint isn't available for Android, but it is open source and writen in C.
So the only logic solution for my problem was to compile libxml2 for Android.
This should be an easy one, because according to the libxml website they only use C ANSI API.<br>
But before I could compile libxml I needed to set up my system running and using the Android NDK.
On this site I want to show how I've done this.
##NDK Overview
---
The Android NDK provides tools to create C/C++ code for Android.<br>
I downloaded the [version r9](http://developer.android.com/tools/sdk/ndk/index.html) for Linux.<br>
I put it into my home folder under 'Tools/android-ndk-r9', so all my settings are made with the condition that the NDK can be found at this location.<br>

