fennecBuild
===========

<pre>
mozilla-beta (prerelease development tree)

When a new release of Firefox enters beta testing, the code is branched into  mozilla-beta. This code represents the expected next release of the Firefox browser, and should be pretty stable. If you want to build off this branch, you can clone the repository as follows:

# Pull the Mozilla source to the folder beta-src/ - may take a while 
# as hundreds of megabytes of history is downloaded to .hg
hg clone http://hg.mozilla.org/releases/mozilla-beta/ beta-src

cd beta-src

パッケージ名、アプリ名の修正
configure.shを書き換え
$ nano mobile/android/branding/unofficial/configure.sh
--------------------
ANDROID_PACKAGE_NAME=com.fennec.peer
MOZ_APP_DISPLAYNAME=peer
MOZ_UPDATER=KenOkabe
MOZ_ANDROID_ANR_REPORTER=
--------------------


copypaste .mozconfig   x86 or arm?
//------------------------


# Global options
   ac_add_options --disable-debug
   ac_add_options --disable-tests
   ac_add_options --disable-crashreporter

 
# Build Fennec
   ac_add_options --enable-application=mobile/android
   ac_add_options --target=arm-linux-androideabi

# Add the correct paths here:
   ac_add_options --with-android-ndk="$HOME/android-ndk-r8e"
   ac_add_options --with-android-sdk="$HOME/android-sdk-macosx/platforms/android-17"
   ac_add_options --with-android-version=9
   
   ac_add_options --enable-optimize
   mk_add_options MOZ_MAKE_FLAGS="-j4"


# DEBUG
ac_add_options --target=i386-linux-android

# RELEASE
# ac_add_options --target=arm-linux-androideabi
# ac_add_options --with-arch=armv6


//------------------------

./mach build

//======== Customize Development Cycle==================

/Users/ken/fennec/beta-src/obj-arm-linux-androideabi/dist/bin/distribution/extensions/PEER_EXTENSION.xpi

./mach package
//========================================================



//========= Development Done -> Release================
Delete 
/Users/ken/fennec/beta-src/obj-arm-linux-androideabi

Switch DEBUG ->RELEASE in .mozconfig
./mach build
/Users/ken/fennec/beta-src/obj-arm-linux-androideabi/dist/bin/distribution/extensions/PEER_EXTENSION.xpi
./mach package



</pre>
