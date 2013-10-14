---
layout: post
title: "Setting up the Android NDK"
description: "How to set up the Android NDK"
category: "Android-NDK"
tags: [Android NDK, Linux]
comments: false
---
{% include JB/setup %}
---
Currently I wanted to make some XPath benchmark tests on my Samsung Galaxy Tab2 10.1 (p5110).<br>
Therefore I needed xmllint to run on Android. Problem here is that xmllint isn't available for Android, but it is open source and written in C.
So the only logic solution for my problem was to compile libxml2 for Android.
This should be an easy one, because according to the libxml website they only use C ANSI API.<br>
But before I could compile libxml I needed to set up my system running and using the Android NDK.
On this site I want to show how I've done this.

---
The Android NDK provides tools to create C/C++ code for Android.<br>
I downloaded the [version r9](http://developer.android.com/tools/sdk/ndk/index.html) for Linux.<br>
I put it into my home folder under 'Tools/android-ndk-r9', so all my settings are made with the condition that the NDK can be found at this location.<br>
Next step was to make some variables and add the compiler location to the path variable.
So I added this lines to my '~/.config/fish/config.fish' file:

    set -gx NDK $HOME/Tools/android-ndk-r9
    set -gx PATH $NDK/toolchains/arm-linux-androideabi-4.8/prebuilt/linux-x86/bin $PATH
After doing this it is possible to compile a C program for Android by just using the Android GCC.<br>
To test this an easy 'Hello World' program would fit.
But I recognized that the given Android date program could neither print milliseconds nor nanoseconds.
So I wrote a small C tool which should do the same as ```$date +%s%N```:
{% highlight c %}
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main() {

	struct timespec *spec;
	spec = (struct timespec *) malloc(sizeof(struct timespec));
	clock_gettime(CLOCK_REALTIME, spec);	
	printf("%d%u\n", spec->tv_sec, spec->tv_nsec);
	free(spec);
	return 0;
}
{% endhighlight %}
To get all needed information I used clock_gettime which is part of time.h.
Time.h is also part of the Android NDK but the compiler needs to know about it, therefore the sysroot has to be set.

    arm-linux-androideabi-gcc nanoseconds.c 
    	--sysroot=$NDK/platforms/android-18/arch-arm/ -o nano
Compiles the program, then create a folder and push it into it:
   
    adb shell mkdir /data/local/tools/
    adb push nano /data/local/tools/
Make it executable and execute it:

    adb shell chmod 777 /data/local/tools/nano
    adb shell /data/local/tools/nano

Now I can compile my C programs for Android, that's what I wanted.
