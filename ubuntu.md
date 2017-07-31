[フリーズした時の対処法](http://hatekun33.hatenablog.com/entry/2014/07/27/023109)


#### suで管理者になろうとしてもパスワードが合わなくて認証失敗になってしまう時

ubuntuではそもそもrootのパスワードは設定されていないらしい

`sudo passwd` で設定できる

`sudo -i`でも管理者になれる（この場合はいつものパスワードでおｋ）

## 日本語入力
```sh
$ sudo apt-get install ibus-mozc
```

再起動

```sh
$ killall ibus-daemon
$ ibus-daemon -d -x &
```

Ubuntuの設定でMozcを追加

<b>※ これでもダメな場合(REMnuxは無理だった)</b>

[http://viva-linux.jp/linux-japanese-garbled-square-232](http://viva-linux.jp/linux-japanese-garbled-square-232)
