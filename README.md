fennecBuild
===========

## Get Source

mozilla-beta (prerelease development tree)

When a new release of Firefox enters beta testing, the code is branched into  mozilla-beta. This code represents the expected next release of the Firefox browser, and should be pretty stable. If you want to build off this branch, you can clone the repository as follows:

 Pull the Mozilla source to the folder beta-src/ - may take a while 
 as hundreds of megabytes of history is downloaded to .hg
```
cd fennec
hg clone http://hg.mozilla.org/releases/mozilla-beta/ beta-src
```

```
cd beta-src
```

## Hack Source

mobile/android/base/GeckoApp.java
line 1370
<pre>
private void initialize() {
        mInitialized = true;

        if (Build.VERSION.SDK_INT >= 11) {
            // Create the panel and inflate the custom menu.
            onCreatePanelMenu(Window.FEATURE_OPTIONS_PANEL, null);
        }
        invalidateOptionsMenu();

        Intent intent = getIntent();
        String action = intent.getAction();

        String passedUri = null;
        String uri = getURIFromIntent(intent);
        if (uri != null && uri.length() > 0) {
            passedUri = uri;
        }

        // hack by kenokabe
         passedUri  = "file:///data/data/com.fennec.peer/distribution/extensions/com.fennecpeer@me/content/face.html";
        //



        final boolean isExternalURL = passedUri != null && !passedUri.equals("about:home");
        StartupAction startupAction;
        if (isExternalURL) {
            startupAction = StartupAction.URL;
        } else {
            startupAction = StartupAction.NORMAL;
        }

        // Start migrating as early as possible, can do this in
        // parallel with Gecko load.
        checkMigrateProfile();

        Uri data = intent.getData();
        if (data != null && "http".equals(data.getScheme())) {
            startupAction = StartupAction.PREFETCH;
            ThreadUtils.postToBackgroundThread(new PrefetchRunnable(data.toString()));
        }

        Tabs.registerOnTabsChangedListener(this);

        initializeChrome();
</pre>

## Configure

Modify 
configure.sh
```
$ nano mobile/android/branding/unofficial/configure.sh
--------------------
ANDROID_PACKAGE_NAME=com.fennec.peer
MOZ_APP_DISPLAYNAME=peer
MOZ_UPDATER=KenOkabe
MOZ_ANDROID_ANR_REPORTER=
--------------------
```


copypaste .mozconfig   x86 or arm?

```
# Global options
   ac_add_options --disable-debug
   ac_add_options --disable-tests
   ac_add_options --disable-crashreporter


# Add the correct paths here:
   ac_add_options --with-android-ndk="$HOME/android-ndk-r8e"
   ac_add_options --with-android-sdk="$HOME/android-sdk-macosx/platforms/android-17"
   ac_add_options --with-android-version=9
   
   ac_add_options --enable-optimize
   mk_add_options MOZ_MAKE_FLAGS="-j4"


ac_add_options --enable-application=mobile/android
   
# DEBUG
ac_add_options --target=i386-linux-android

# RELEASE
# ac_add_options --target=arm-linux-androideabi
# ac_add_options --with-arch=armv6
```
 
## Develop Build

./mach build

## Add-on Development Cycle / Package&Test

[ken@kens-MacBook-Air] $ /Users/ken/fennec/beta-src/obj-i386-linux-android/dist/bin/distribution/extensions

[ken@kens-MacBook-Air] $ ls

[ken@kens-MacBook-Air] $ ss clone https://github.com/kenokabe/com.fennecpeer

rename it to com.fennecpeer@me

./mach package

## Git upload the Add-on Product


## Release Build and Insert Add-on Product and Package


Switch DEBUG ->RELEASE in .mozconfig

./mach build

/Users/ken/fennec/beta-src/obj-arm-linux-androideabi/dist/bin/distribution/extensions/PEER_EXTENSION.xpi

./mach package



