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
