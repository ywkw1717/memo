```
~/hogehoge/fugafuga :yyy@+[feature/hoge-111](*｡˃̵ω ˂̵｡) < ./bin/rails s
=> Booting Puma
=> Rails 5.1.3 application starting in development on http://localhost:3000
=> Run `rails server -h` for more startup options
FATAL: Listen error: unable to monitor directories for changes.
Visit https://github.com/guard/listen/wiki/Increasing-the-amount-of-inotify-watchers for info on how to fix this.
Exiting
~/hogehoge/fugafuga :yyy@+[feature/hoge-111](๑˃̵ᴗ˂̵)ﻭ < cat /proc/sys/fs/inotify/max_user_watches
8192
```

Solution: [https://github.com/guard/listen/wiki/Increasing-the-amount-of-inotify-watchers](https://github.com/guard/listen/wiki/Increasing-the-amount-of-inotify-watchers)
