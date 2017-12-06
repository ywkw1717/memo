About plt
```
セクション .plt の逆アセンブル:

08048580 <printf@plt-0x10>:
 8048580:	ff 35 04 a0 04 08    	push   DWORD PTR ds:0x804a004 <- 0x804a004には最初は０が格納されている。その後、特定のライブラリが使われているかを識別するための番号になるらしい
 8048586:	ff 25 08 a0 04 08    	jmp    DWORD PTR ds:0x804a008 <- 0x804a008にも最初は０が格納されている。その後、リンカーのシンボル解決ルーチンのアドレスになるらしい
 804858c:	00 00                	add    BYTE PTR [eax],al
	...

08048590 <printf@plt>:
 8048590:	ff 25 0c a0 04 08    	jmp    DWORD PTR ds:0x804a00c <- 0x804a00cには最初は0x8048596が入っていて、アドレス解決されると共有ライブラリ本体の中にあるprintfのアドレスが格納される
 8048596:	68 00 00 00 00       	push   0x0 <- 再配置テーブルへのオフセットアドレス
 804859b:	e9 e0 ff ff ff       	jmp    8048580 <_init+0x28>

080485a0 <fgets@plt>:
 80485a0:	ff 25 10 a0 04 08    	jmp    DWORD PTR ds:0x804a010
 80485a6:	68 08 00 00 00       	push   0x8
 80485ab:	e9 d0 ff ff ff       	jmp    8048580 <_init+0x28>
```

[https://ledyba.org/2014/06/13093609.php](https://ledyba.org/2014/06/13093609.php)
