install
```sh
sudo apt-get isntall redis-tools
```

redis-cli
```sh
redis-cli -h greeneye.redistogo.com -p 11144 -a 19f4ff1dc568b5848a019f8965f11167
```

key verification
```sh
keys *
type hubot:storage
```

if type is string
```sh
get hubot:storage
```
