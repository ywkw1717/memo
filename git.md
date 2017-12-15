### stash-memo

git stashは便利
変更内容を一時退避できる
ある作業が中途半端な状態になっているときに、ブランチを切り替えて少しだけ作業をしたくなる時
中途半端な状態をコミットしてしまうのは嫌、できればコミットせずにしておいて後でその状態から作業を再開したい
そこでgit stashコマンド
未完了の作業をスタックに格納し、あとで好きなときにサ再度それを適用できるようにするもの

```
git stash listで退避状況を見れる
git stash show stash名で変更内容を見れる
git stash clear でlistをclear
git stash applyで退避した内容を再度適用
名前を指定しなければ直前の内容を適用
git stash stash@{2}のように名前を指定すればそれを適用
```

ファイルへの変更は元通りだが、以前にステージしていたファイルはステージされていない（addとかcommitとかの話）
それを行うには、git stash apply --indexで変更のステージ処理も再適用できる

applyオプションは、スタックに退避した作業を再度適用するだけで、スタックにはまだその作業が残ったままになる
よってスタックから削除するには、git stash dropに削除したい作業の名前を指定して実行(指定しなければ直前の内容を削除）


### config-memo

```sh
git config --local user.name NAME
git config --local user.email EMAIL@hogehoge.com
```

で各リポジトリ毎に変更

```sh
git remote add origin URL
```

でoriginを[設定](http://qiita.com/masakinihirota/items/4fe8596a76adeb6a8cbf)


[GitのCommitのAuthorとComitterを変更する方法](http://qiita.com/sea_mountain/items/d70216a5bc16a88ed932)

### commit-memo

```sh
git filter-branch --commit-filter '
    GIT_AUTHOR_NAME="yyy"
    GIT_AUTHOR_EMAIL="y.w.k.w.1717@gmail.com"
    GIT_COMMITTER_NAME="yyy"
    GIT_COMMITTER_EMAIL="y.w.k.w.1717@gmail.com"
    git commit-tree "$@"
' HEAD
```

```sh
git push -f origin master
```

## command
- git branch --contain <commit hash>
- git show-branch <commit hash>
