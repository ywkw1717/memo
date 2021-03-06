## Reverse engineering APK file
```sh
$ unzip hoge.apk
$ java -jar AXMLPrinter2.jar /path/to//AndroidManifest.xml > ConvertedManifest.xml
$ /path/to/d2j-dex2jar.sh /path/to/classes.dex
$ jd-gui
```

## /libのファイルを改変してリビルドしたい場合（改造？）

apk studio でビルドしてから↓ で署名

Passphraseはandroid

Generate keystore
```sh
$ keytool -genkey -v -keystore debug.keystore -alias androiddebugkey -keyalg RSA -validity 10000 -dname "CN=Android Debug,O=Android,C=US"
```

署名には` -sigalg SHA1withRSA -digestalg SHA1`が必要らしい

[http://qiita.com/hishida/items/8ab56f4780fbfaf4811f#comment-08a24c55bd18bf125c38](http://qiita.com/hishida/items/8ab56f4780fbfaf4811f#comment-08a24c55bd18bf125c38)

Passphraseはandroid

```sh
$ jarsigner -keystore debug.keystore -verbose HelloWorld-release-signed.apk androiddebugkey -sigalg SHA1withRSA -digestalg SHA1
```

署名確認
```sh
$ jarsigner -verify HelloWorld-release-signed.apk
```

## Genymotion
`Genymotion-ARM-Translation_v1.1`をドラックアンドドロップしてからapkファイルをドラックアンドドロップする必要がある


## Apkstudio Install
```sh
$ sudo apt-get install qt5-default qt5-make
$ git clone https://github.com/vaibhavpandeyvpz/apkstudio.git
$ cd apkstudio
$ lrelease res/lang/en.ts
$ qmake apkstudio.pro
$ make
```

## root検知手法
- /system/app/Superuser.apkの有無

- ro.build.tagsがtest-keysかどうか

- 特定のパッケージの有無
  - com.noshufou.android.su
  - com.thirdparty.superuser
  - eu.chainfire.supersu
  - com.koushikdutta.superuser
  - com.zachspong.temprootremovejb
  - com.ramdroid.appquarantine

- /system/bin/su や /system/xbin/su など、suの有無

- idコマンドを実行してuidが０かどうか、(root)が含まれているかどうか

- busyboxコマンドの実行

- default.propの値のチェック
  - ro.secure=0
  - ro.allow.mock.location=1
  - ro.debuggable=1
  - persist.sys.usb.config=adb

### [reference]
[https://blog.netspi.com/android-root-detection-techniques/](https://blog.netspi.com/android-root-detection-techniques/)

[http://minkara.carview.co.jp/userid/2500523/blog/36484980/](http://minkara.carview.co.jp/userid/2500523/blog/36484980/)


## 端末のABIの特定
```sh
$ getprop |grep cpu
```

## BlueStacks
Genymotionはx86なので、arm製のアプリはこっちで動かす

### root化
Bluestacks easyとかいうのを使うより、[https://www.youtube.com/watch?v=HHPLXNzkVyw](https://www.youtube.com/watch?v=HHPLXNzkVyw) こっちでやったほうが簡単だし上手く行く


## adbでviがバグる現象
puttyを使う方法がネット上で多かったけど、BlueStacksでは上手く動かなかった。なのでpowershellを使う。

powershellからadbを使うとバグが起きない、神

## Cheat Engine
[インストールと使い方](http://blog.livedoor.jp/kiyopen/archives/52368327.html)

確か、署名なしドライバをインストールできないとか言われて、UEFIのセキュアブートを無効にしたり、いろいろやることがあった。
