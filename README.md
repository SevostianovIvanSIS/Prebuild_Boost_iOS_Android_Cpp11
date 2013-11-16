Prebuild_Boost_iOS_Android_Cpp11
================================

Prebuild boost for iOS/Android (boost_1.54 and boost_1.53) With C++11 enabled

This repo is practically a fork of https://github.com/mgrebenets/boost-ios-xcode5 and 
https://github.com/MysticTreeGames/Boost-for-Android

———————————————————————————————————————————————————————————————————————————————————————

Boost for Android
Boost for android is a set of tools to compile the main part of the Boost C++ Libraries for the Android platform.

Currently supported boost versions are 1.45.0, 1.48.0, 1.49.0 and 1.53.0.

To compile Boost for Android you may use one of the following NDKs:

NDK / boost	1.45	1.48	1.49	1.53
r4 customized by Dmitry Moskalchuk aka CrystaX.	x			
r5 from the official android repository.	x			
r5 customized by CrystaX.	x			
r7 customized by CrystaX.	x	x	x	
r8 from the official android repository.	x	x	x	
r8b from the official android repository.		x	x	
r8c from the official android repository.			x	
r8d from the official android repository.			x	x
r8e from the official android repository.			x	x
Quick Start
Dependencies

NDK (official or customized by CrystaX)
GNU Make
Usage

$ ./build-android.sh $(NDK_ROOT)
This command will download and build boost against the NDK specified and output the final headers and libs and in the build folder.

For more info about usage and available commands use --help

Now that you got boost compiled you must add it to your Android.mk file. First copy the inlcude and lib filder over to your jni folder. I copied it just into: /jni/boost/.

Add the following to your Android.mk (example for boost 1.48):

LOCAL_CFLAGS += -I$(LOCAL_PATH)/boost/include/boost-1_48
LOCAL_LDLIBS += -L$(LOCAL_PATH)/boost/lib/ -lboost_system -lboost_...

LOCAL_CPPFLAGS += -fexceptions
LOCAL_CPPFLAGS += -frtti
Now use ndk-build and have fun with it! Also note that you should build your project and Boost with one version of NDK - STL inside NDK r4 and NDK r5 are not compatible in some subtle details.

Troubleshooting

In case you encounter bunch of linker errors when building your app with boost, this might help:

Building from a 64 bit machine (linux)

Make sure you have installed the 32 bit libraries. Those are required to be able to use the NDK.

To install them just use the following

$ sudo apt-get install ia32-libs
NDK 7 (CrystaX)

Add -lgnustl_static AFTER all boost libraries to the LOCAL_LDLIBS line in Android.mk. Example:

LOCAL_LDLIBS += lboost_system-gcc-md lboost_thread-gcc-md -lgnustl_static
NDK 8 (official)

Do everything that is in the NDK 7 Crystax section, but also add full path to the gnustl_static library to the link paths. Example:

LOCAL_LDLIBS += lboost_system-gcc-md lboost_thread-gcc-md \
             -L$(NDK_ROOT)/sources/cxx-stl/gnu-libstdc++/libs/armeabi \
             -lgnustl_static

———————————————————————————————————————————————————————————————————————————————————————
Build Boost Framework for iOS and OSX
Using Xcode5 (armv7, armv7s, arm64, i386, x86_64)

Boost Source

Script does not yet support downloading the source code, so you have to get it manually.

boost downloads
1.53.0
1.54.0
The script is expecting bz2 tarball. Put the tarball in the same folder with boost.sh.

Make sure you keep the tarball name unchanged, so it is like boost_1_53_0.tar.bz2.

Build

Use boost.sh to build boost framework. Run boost.sh -h to get help message.

Modify BOOST_LIBS with list of libraries that you need.

Examples:

# clean build version 1.53.0 for ios and osx with c++11
./boost.sh clean --with-c++11 -v 1.53.0

# build version 1.54.0 for ios and osx without c++11, no clean
./boost.sh --version 1.54.0
Notes and Changes

Link Errors in Xcode 5

If you use libraries like serialization you might see link errors in Xcode 5 especially when the framework was built using --with-c++11 flag.

You have to change your project or target build settings.

Under Apple LLVM 5.0 - Language - C++ make the following changes

C++ Language Dialect set to C++11 [-std=c++11]
C++ Standard Library set to libc++ (LLVM C++ standard library with C++11 support)
ar for Simulator Dev Tools

In Xcode 5 there's no ar excutable in SIM_DEV_DIR so using /usr/bin/ar instead.

Why not Using Cocoapods?

I tried to use cocoapods spec for boost. However, there's a number of things that made me to switch to using framework instead.

It doesn't include all the subspecs you might need for development
It takes really long time to update every time you run pod update or pod install (given that you have modified Podfile)
The tar-ball is downloaded (50+ mb)
The tag-ball is unpacked
There's podspec for 1.51.0 only
You can't use the Pod if you need libraries like serialization
serialization has to be linked as a library, it doesn't work like the rest of the boost libraries by just including hpp headers inline. It needs to be compiled for your target platform.

References and Attribution

The script mentioned above in it's turn is based on great work by Pete Goodliffe

https://gitorious.org/boostoniphone
http://goodliffe.blogspot.com.au/2010/09/building-boost-framework-for-ios-iphone.html
http://goodliffe.blogspot.com.au/2009/12/boost-on-iphone.html
And lots of contributions by other people.

———————————————————————————————————————————————————————————————————————————————————————
In prebuildBoost folder your can find prebuilt boost for Android and iOS with C++11 enabled 
for iOS build libs : 
-atomic
-chrono
-context
-coroutine
-date_time
-exception
-filesystem
-graph
-graph_parallel
-iostreams
-locale
-log
-math
-mpi
-program_options
-python
-random
-regex
-serialization
-signals
-system
-test
-thread
-timer
-wave 

for Android build libs :
-atomic
-chrono
-date_time
-exception
-filesystem
-graph
-graph_parallel
-iostreams
-locale
-math
-mpi
-program_options
-python
-random
-regex
-serialization
-signals
-system
-test
-thread
-timer
-wave