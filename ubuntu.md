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

## apt-file
```sh
$ sudo apt-get install apt-file
```

```sh
$ sudo apt-file search HOGE
```

apt-cache search と一緒？

## gnome-terminalのタブ色変更
```sh
$ touch ~/.config/gtk-3.0/gtk.css
```

gtk.css
```css
TerminalWindow .notebook tab:active {
  background-color: #def;
}
```

## GitHubのSSHでPermission deniedになる問題
```
Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

今回の場合、sshdが自動起動になっていなかったことが原因？→自動起動にしてもダメっぽい

とりあえず、以下で直る

```
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_bitbucket_nowhere_rsa
```
