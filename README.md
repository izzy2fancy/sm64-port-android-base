# sm64ex Android Port
This is a port of the reconstructed Super Mario 64 source code to Android using SDL2 with OpenGL ES 2.0.

It has cross-platform Touch Controls, Audio works, it saves the game to the app's internal storage and you can play it with an external keyboard or controller as well (tested on PS3 controller).

# Build instructions

## Linux

**Install dependencies:**

This depends on your distro, but if you can build the PC port and you have Android SDK/NDK and you are able to build Android apps using gradle, you should be fine.

**Clone the repository:**
```sh
git clone --recursive https://github.com/VDavid003/sm64-port-android-base --branch sm64ex
cd sm64-port-android-base
```

**Copy in your baserom:**
```sh
cp /path/to/your/baserom.z64 ./app/jni/src/baserom.us.z64
```

**Get SDL sources:**
```sh
./getSDL.sh
```

**Perform native build twice:**
```sh
# if you have more cores available, you can increase the --jobs parameter
cd app/jni/src
make --jobs 4
make --jobs 4
cd ../../..
```

**Perform Android build:**
```sh
./gradlew assembleDebug
```

**Enjoy your apk:**
```sh
ls -al ./app/build/outputs/apk/debug/app-debug.apk
```

## Windows

**Install dependencies:**

You'll need everything you need to make Windows builds (not just vanilla sm64 ones, but sm64ex ones), and to be able to build Android apps using `gradlew.bat`. This includes Java JDK (with the JDK being JAVA_HOME) and Android SDK/NDK. Every commmand is executed in MSYS2 unless otherwise noted.

**Clone the repository:**
```sh
git clone --recursive https://github.com/VDavid003/sm64-port-android-base --branch sm64ex
```

**Copy in your baserom:**
Use the file explorer, or whatever you want, just put it in `app/jni/src`, and name it like you'd do on the PC port.
```sh
cp /path/to/your/baserom.z64 ./app/jni/src/baserom.us.z64
```

**Get SDL sources:**
```sh
./getSDL.sh
```

**Perform native build twice:**
```sh
# if you have more cores available, you can increase the --jobs parameter
cd app/jni/src
make --jobs 4
make --jobs 4
cd ../../..
```

**Perform Android build:**
Do this in a normal Command Prompt!
```
gradlew.bat assembleDebug
```

## Docker

**Clone the repository:**
```sh
git clone --recursive https://github.com/VDavid003/sm64-port-android-base --branch sm64ex
```

**Create the build image:**
```sh
# navigate into newly cloned repo
cd sm64-port-android-base
# build the docker image
docker build . -t sm64_android
```
**Copy in your baserom:**
```sh
cp /path/to/your/baserom.z64 ./app/jni/src/baserom.us.z64
```

**Setup symlinks for SDL:**
```sh
docker run --rm -v $(pwd):/sm64 sm64_android sh -c "ln -nsf /SDL2-2.0.12/src /sm64/app/jni/SDL/src"
docker run --rm -v $(pwd):/sm64 sm64_android sh -c "ln -nsf /SDL2-2.0.12/include /sm64/app/jni/SDL/include"
```

**Perform native build twice:**
```sh
# if you have more cores available, you can increase the --jobs parameter
docker run --rm -v $(pwd):/sm64 sm64_android sh -c "cd /sm64/app/jni/src && make --jobs 4"
docker run --rm -v $(pwd):/sm64 sm64_android sh -c "cd /sm64/app/jni/src && make --jobs 4"
```

**Perform Android build:**
```sh
docker run --rm -v $(pwd):/sm64 sm64_android sh -c "./gradlew assembleDebug"
```

**Enjoy your apk:**
```sh
ls -al ./app/build/outputs/apk/debug/app-debug.apk
```

# Configuration
If you want to customize the build with build options, you should make the native build with those options first (put them after the make command like on normal repos), then before performing the Android build, edit `app/jni/src/Android.mk` and enable the options you'd like.

## EXTERNAL_DATA option
If you use `EXTERNAL_DATA`, you'll find a zip named `base.zip` in `app/jni/src/build/<version>_pc/res`.

You should take this zip and put it in `Internal Storage/Android/data/com.vdavid003.sm64port/files`
