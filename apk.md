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