## Android SDK
```sh
$ wget https://dl.google.com/android/repository/sdk-tools-linux-3859397.zip
$ unzip sdk-tools-linux-3859397.zip
$ mkdir ~/android-sdk-linux
$ mv tools ~/android-sdk-linux
$ cd ~/android-sdk-linux
$ android update sdk
```

```sh
$ export ANDROID_HOME=$HOME/android-sdk-linux
$ export PATH=$PATH:$ANDROID_HOME/tools
$ export PATH=$PATH:$ANDROID_HOME/platform-tools
```

## Android NDK
```sh
$ wget http://dl.google.com/android/ndk/android-ndk-r10d-linux-x86_64.bin
$ chmod a+x android-ndk-r10d-linux-x86_64.bin
$ ./android-ndk-r10d-linux-x86_64.bin
$ sudo mv android-ndk-r10d /usr/local
```

```sh
$ export ANDROID_NDK_HOME=/usr/local/android-ndk-r10d
$ export PATH=${PATH}:$ANDROID_NDK_HOME
```

## About "update project" command
[https://askubuntu.com/questions/906942/android-update-project-path-target-android-25-on-ubuntu-16-04](https://askubuntu.com/questions/906942/android-update-project-path-target-android-25-on-ubuntu-16-04)
