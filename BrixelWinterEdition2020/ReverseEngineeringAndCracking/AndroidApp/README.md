# Android App
**Category:** [Reverse Engineering/Cracking](../README.md)

**Points:** 20

**Description:**

This little android app requires a password, can you find it?

the flag is the password

**Files:** brixelCTF.apk

## Write-up
The attached file was an Android APK file. We can disassemble this with [Apktool](https://ibotpeaches.github.io/Apktool/). We downloaded *apktool_2.5.0.jar* and ran it over the *brixelCTF.apk* file.
```
> java -jar .\apktool_2.5.0.jar decode .\brixelCTF.apk
I: Using Apktool 2.5.0 on brixelCTF.apk
I: Loading resource table...
I: Decoding AndroidManifest.xml with resources...
I: Loading resource table from file: C:\Users\chris\AppData\Local\apktool\framework\1.apk
I: Regular manifest package...
I: Decoding file-resources...
I: Decoding values */* XMLs...
I: Baksmaling classes.dex...
I: Baksmaling classes2.dex...
I: Copying assets and libs...
I: Copying unknown files...
I: Copying original files...
```
This created a new directory named *brixelCTF* containing the decompiled Android app.

We tried running the app using [BlueStacks](https://www.bluestacks.com/). This didn't help.

We loaded the app into [Android Studio](https://developer.android.com/studio). There were a lot of files, and it was difficult to work though, but we searched for `brixelCTF{` to see if we could find the flag. We did!

> Note: The flag can also be found with `grep -r "brixelCTF{" brixelCTF`
