Only syn packet(host)
```sh
$ tshark -r filename -Y 'tcp.flags.syn==1' |awk '{print $3}'
```

Only smb negotiate request
```sh
$ tshark -r filename -Y smb.cmd == 0x72 and (smb.flags.response == 0)
```
