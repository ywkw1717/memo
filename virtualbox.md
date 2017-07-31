## 共有フォルダ
作ってから、

```sh
$ sudo mount -t vboxsf DIRECTORY_NAME /mnt/DIRECTORY_NAME
```

## Install Guest Additions

```sh
$ sudo mount -r /dev/cdrom /media
$ cd /media
$ sudo ./VBoxLinuxAdditions.run
```

After install finished

```sh
$ cd
$ sudo umount /media
$ sudo shutdown -r now
```
