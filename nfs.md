サーバ側
$ sudo apt-get install nfs-kernel-server
/etc/exportsに以下を追記

# (公開したいディレクトリ)(どれに公開するか)(公開モード)
/home/hoge/fuga 10.10.10.11(rw,sync,no_subtree_check)

# 設定を反映
$ sudo systemctl restart nfs-kernel-server

クライアント側
$ sudo apt-get install nfs-common
$ mkdir /mnt/hogehoge  # マウント先を作成

$ sudo mount -t nfs 10.10.10.10:/home/hoge/fuga /mnt/hogehoge


自動マウントしたい場合は/etc/fstabに追記
#(マウントしたいディレクトリ)(マウント先) nfs rw 0 0
10.10.10.10:/home/hoge/fuga /mnt/hogehoge nfs rw 0 0

[https://qiita.com/salty-vanilla/items/5bc95ad47af6bf051bf7](https://qiita.com/salty-vanilla/items/5bc95ad47af6bf051bf7)
[https://www.mk-mode.com/octopress/2015/06/08/debian-8-nfs-installation/](https://www.mk-mode.com/octopress/2015/06/08/debian-8-nfs-installation/)
