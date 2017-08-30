## sudo gem: not found 的な

sudo時にPATHが引き継がれていない

```
sudo visudoでDefault secure_path = ...
```

のところに追加したいPATHを追加

## ソースからビルド
tarballを展開してから

```sh
$ ./configure
$ make
$ sudo make install
```

.localにインストールする場合
```sh
$ ./configure --prefix=/home/hogehoge/.local
$ make
$ make install
```
PATHを通しておわり

## PDF作成
[Prawn](https://github.com/prawnpdf/prawn)というgem

[http://takuya-1st.hatenablog.jp/entry/2015/01/18/223308](http://takuya-1st.hatenablog.jp/entry/2015/01/18/223308)

[http://prawnpdf.org/manual.pdf](http://prawnpdf.org/manual.pdf)


## rbenvのupdate
2.4.1を入れたかったが、`rbenv install --list`で、一覧になかった

```shell
$ sudo apt-get install ruby-build
$ mkdir -p "$(rbenv root)/plugins"
$ git clone https://github.com/rkh/rbenv-update.git "$(rbenv root)/plugins/rbenv-update"
$ rbenv update
$ rbenv install 2.4.1
```
