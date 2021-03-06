# Reversing
表層解析 → 動的解析 →　静的解析

## 表層解析
ファイルのフォーマットを特定や、文字列を抽出

（圧縮されていれば解凍が必要だし、実行可能ファイルでなければ、バイナリとは違う分野の技術が必要かもしれない。また実行ファイルだった場合は、どのような形式なのか、どのような環境で動くのか、静的リンクなのか、動的リンクなのか、stripされているのかなど）をfileコマンドで

- "P32 executable"
PE（Portable Executable）フォーマット:Windows上で用いられる実行ファイル形式

（GUI）の表示 →　GUIを使うことがわかる

"Intel 80386, for MS Windows"の部分は、対応しているCPUアーキテクチャとOS　それぞれ、i386,Windowsに向けたものであることがわかる


- "ELF 32-bit LSB executable"
32bit OS向けのELFフォーマットの実行ファイル

ELFフォーマットは主にLinuxやBSDベースのOSで用いられる実行ファイル形式

LSBとは、Least Significant Byteのことで、ここでは、エンディアンがリトルエンディアンであることを指している


- "Intel 80386"
i386向け


- "dynamically linked(uses shared libs)"
この実行ファイルが動的リンクを採用していることを指す


- "not stripped"
実行ファイル内のシンボル情報が削除されずに残っていることを指す（stripというコマンドがLinuxにあるし、そういうこと？）

- "statically linked"
静的リンク

- "stripped"
シンボル情報が削除されている

シンボル情報とは、関数名やアドレスなどを示したもの（stripコマンドで実現される）

シンボル情報が削除されていることがどう解析に影響を与えるかについては、後ほど


## [動的解析]
#### メリット
- 少ない時間で情報が得られる場合が多い
- 解析に要する知識が、静的解析と比較して少なくて済む

#### デメリット
- 理論上はプログラムのほぼすべての動作が把握できる静的解析と比較して、動的解析は詳細な動作の情報が得難い

動的解析によって、特に注力して解析するべき部分に焦点を当て、詳細なアルゴリズムを読み解く必要がある部分などは、静的解析で一命令単位で解析するのが良いかも

#### 「実行による動作の確認」
どのようなフローで入出力が行われているかを確認することは重要な手がかりになる

実際に動かす際に注意すること：できる限り仮想環境を用いる

CTFのバイナリ問題の中には、マルウェアのような動作をするものも存在する

fork bomb: 新たなプロセスを次々に生成させることで、プロセス数の上限に達させ、これ以上プロセスが生成できない状態にさせるDos攻撃の一種

#### 「システムコール・ライブラリコールのトレース」
トレーサを通した実行では、通常の実行と比較すると多くの情報を得ることができる

情報がシステムコールやAPIコールの形で得られるため、プログラミングの知識さえあれば、アセンブリに詳しくなくても、トレース結果を理解できる

- strace
forkをして、子プロセスは生成されてすぐに終了し、親プロセスは生成した子プロセスのプロセスID（PID）を標準入力から入力させてチェックする、という問題

子プロセスのPIDを知るために、straceでシステムコールをトレースしながら実行してみる

→ システムコールの呼び出しを、引数と返り値を伴って追うことができている、子プロセスを生成するシステムコールはcloneであり、cloneの返り値は生成された子プロセスのPIDとなっている

以上のように、straceで実行することによって、プログラムの実行時にどのようなシステムコールが呼ばれたか、渡された引数や、返り値と共に、追跡できる


- ltrace
入力文字列と、ハードコードされたkeyとなる文字列とをstrcmpで比較し、一致した場合にFLAGを表示する問題

入力値をチェックする問題としては、最も簡単なものの一つ

正しいパスフレーズを手に入れることを目指す

ltraceでトレースしながら実行

ltraceは標準ライブラリ関数の呼び出しをトレースする

straceのように、プログラムの実行の際に呼び出された標準ライブラリ関数をその引数と返り値がわかる形で追えているのがわかる

ltraceで標準ライブラリ関数の呼び出しや引数、返り値を追うことによって、プログラムの動作をある程度把握することが可能

パスフレーズやFLAGが暗号化やエンコードされていない時はstringsで手に入れることが可能だが、暗号化やエンコードされていた場合は、ltraceを用いれば、strcmpに引数とし渡される際のパスフレーズは、必ず復号やデコードされているはずなので、解ける

- ※注意点
マルチプロセスのプログラムを実行する際に、親プロセスと子プロセスのどちらをトレースするか、ということがある

CTFでは、サーバとして通信を行うようなバイナリを解析する問題の場合などは、forkなどを用いたマルチプロセスが採用されていて、子プロセスをトレースしなければならない場合が多い

そのような時は、-fオプションをつける（strace,ltraceのどちらも）ことで、子プロセスのみを選択的に追わせることが可能

トレーサを用いることの

#### メリット
- 解析に大きな手間をかけずに、バイナリの大まかな挙動を知ることができる

#### デメリット
- これらのトレーサでは、システムコールやライブラリコールを用いずに実現されている挙動は把握できないという点

命令を複雑に組み合わせて自前でアルゴリズムを実現している場合も少なくないため、注意が必要

それらを解析するためには、一命令ずつ命令列を読み、解釈していく力が必要になる

#### まとめ：
トレーサは全体の挙動を概観したり、注力して命令を読むべき場所を特定したりなど、本格的な解析の前段階として用いるのが良い

### 「デバッガでの実行」
動的解析で最も多くの情報を得られる手法の一つ、デバッガでの解析

- デバッギー： デバッガで解析する対象
CTFでは、ソースコードがないことがほとんどなので、アセンブリ命令レベルでブレークポイントを仕掛ける

- シングルステップ実行：命令やコード行などを単位としたデバッグで、一単位分実行を進めるもの
↓
- ステップオーバー（ステップ実行）

- ステップイン（詳細ステップ実行）

の２つがある

- ステップオーバー：call命令によるサブルーチンの呼び出しがあった時、それを一命令として捉える。つまり、call命令の呼び先まで入ってステップ実行することはなく、call命令の次の命令へと移る

- ステップイン：call命令の呼び先に飛んで、一命令ずつ実行する

関数の中身まで解析したいときは、必ずステップインを使う

一方、標準ライブラリ関数のように、既に動作がわかっていて解析する必要があまりないものまでステップインしてしまうと、効率が悪くなる

ブレークした状態で、レジスタやスタック、メモリの状態を確認することで、様々な情報を得ることができる


実行制御、実行状態監視の繰り返しによって、デバッギーの様々な情報を得るということが、デバッガによる解析の本質

### OllyDbg
- 逆アセンブラウィンドウ
- レジスタウィンドウ
- メモリダンプウィンドウ
- スタックダンプウィンドウ

- OllyDbgでの解析の基本
逆アセンブルウィンドウの命令をある程度理解しながらブレークポイントやステップ実行によって実行を進め、レジスタの監視やレジスタ値を変更して、デバッギーがどのように動作するかを確認する

### -gdb編-
- レジスタ名の指定
$eaxのように、、$をプレフィックス（接頭辞）として付加

- メモリアドレスの参照
C言語のポインタ変数のように、*0x80000000のように、*をプレフィックスとして付加

- 起動
gdbコマンドにデバッギーのファイル名を与えて実行

現在実行中のプロセスに対してデバッグしたい場合はｋ，-pオプションを用いてデバッギーのPIDを与える

- 逆アセンブルの表示
disas (関数名)　で関数内全てを逆アセンブルして表示

disas (関数内での開始アドレス) (関数内での終了アドレス)　でその範囲を逆アセンブルして表示

- 関数内に含まれていない領域を逆アセンブルする場合
xコマンドを使う

x/ (命令数) i (先頭アドレス)　で先頭アドレスから、指定した命令数分、逆アセンブルする

- アセンブリ言語の記法の変更
gdbはデフォではAT&T記法

Intel記法/AT&T記法の切り替えは

Intel記法：set disassembly-flavor intel

AT&T記法： set disassembly-flavor att

- ブレークポイントのセット
b (関数名)

b (アドレス)

bはbreakのb

- ブレークポイントの状態の表示
ブレークポイントがどこにセットされているかなどの、状態を確認する場合

i b

(info breakの略)

実際にブレークポイントを仕掛けて実行してみると、Num, Type, Disp,.....などのカラムがある

一番のNumは、ブレークポイントに対して割り振られる番号で、この番号を用いて、削除、無効化、有効化などの管理をする

- ブレークポイントの削除、無効化、有効化
- 削除
d (ブレークポイントの番号)

(deleteの略)

- 一時的に無効化
disable (ブレークポイントの番号)

- 無効になっているものを有効化
enable (ブレークポイントの番号)

- デバッギーの実行
r

- コマンドライン引数を渡す場合
r (コマンドライン引数１) (コマンドライン引数２) ...

(runの略称)

- main関数でブレークさせる場合は、startコマンド
start

- コマンドライン引数を渡す場合
start (コマンドライン引数１) (コマンドライン引数２) ...

また、コマンドライン引数ば以下のようにして予めセットしておくこともできる

set args (コマンドライン引数１) (コマンドライン引数２) ...

- setコマンド
以下のようにしてレジスタに値をセットする用途にも使える

set (レジスタ名)=(値)

set $pc=0x80000000とすることで、命令ポインタを変更することができる、これは条件分岐先を強制的に変更する際など、様々なケースで利用

- ステップ実行（ステップオーバー）
ni

(nextiの略称)

これによって、一命令分、実行することができる

もちろんステップオーバーだから、続く命令がサブルーチン呼び出しの場合もサブルーチンの中は実行しないで、call命令も一命令として扱われる

- 詳細ステップ実行（ステップイン）
si

(stepiの略称)

続く命令がサブルーチンの場合は、サブルーチンの中に入って実行を続ける

- cコマンド
次のブレークポイントまで動作を継続する場合

c

(continueの略称)

- レジスタの値の読み出し
p (レジスタ名)

- i r
（info registersの略称）

とすると、一部のレジスタを除く全てのレジスタの値を表示させることができる

- メモリダンプ
メモリの内容を表示したい場合は、xコマンド

若干変速的な書き方をするコマンドで、表示する数、表示するデータのサイズ、表示するメモリの先頭アドレスをとる

x/(表示する数)(メモリサイズ)(表示フォーマット)(表示するメモリの先頭アドレス)

- データのサイズは以下
*b:Byte(1バイト)

*h:HALFWORD(2バイト)

*w:WORD(4バイト)

*g:GIANTWORD(8バイト)

- 表示フォーマット
\*s:文字列
\*i:命令
\*x:１６進数

```
x/100wx *0x4010000
```

と書くと、0x04010000のアドレスから、4バイトで100個のメモリを16進数値として読みだす

- スタック関連
backtrace
(または、bt)

- コールスタックのバックとレースを確認
現在の命令ポインタの位置に至るまで、どのような関数を経ているかを表示してくれる

例えば、現在、関数Aから関数Bが呼び出され、さらに関数Bから呼び出される関数Cの中でブレークしている場合、これらの関数を表示してくれる

- ウォッチポイント関連
ウォッチポイントという機能を使うと、指定したメモリ領域の書き換えや読み取りがあった時にブレークさせることができる

- watch (メモリアドレス)
watchコマンドでは指定したメモリアドレスへの書き換えを検出してブレーク

- rw (メモリアドレス)
（rwatchの略称）

rwコマンドでは読み取りを検出してブレーク

- aw (メモリアドレス)
（awatchの略称）

awコマンドでは読み書きの両方を検出してブレーク


## [静的解析]
バイナリのプログラムコードを読むことで動作を知り、解析を実施する手法

逆アセンブルによって、アセンブリ言語に変換したものを読み解くのが一般的

静的解析手法によって、理論上はバイナリの持つ全ての動作を解析可能

だが、動的解析と比較しても、非常に時間がかかる

### レジスタとスタック
アセンブリ言語を読み解く上で欠かせない概念であるレジスタとスタック

アセンブリ言語は、機械語と一対一に対応している

アセンブリ言語の命令群がどのように設計されているかは、コンピュータの設計と密接に関わっていると言える

プロセッサのアーキテクチャ（基本となる設計のこと）によって、どのような命令が用意されているかが、変わってくるため、アセンブリ命令を理解するためには、プロセッサのアーキテクチャや、プロセッサの用いるデータ構造をよく理解する必要がある

- x86アーキテクチャ：現在、汎用コンピュータにおいて最もよく用いられているプロセッサアーキテクチャ

#### レジスタ
プロセッサが命令を実行する際には、直接メモリを操作するのではなく、メモリからレジスタに読みだしたデータに対して操作をすることが多くなっている

記憶容量自体は、メモリと比較しても小さく、32bitや64bit程度のものが多い

プロセッサについて、一般に32bit CPU、 64bit CPUなどと呼称することがあるが、これは、レジスタ幅によって決められている

プロセッサは大抵複数のレジスタを持ち、用途によって使い分けている

#### x86アーキテクチャの持つレジスタについて説明
x86アーキテクチャには、

EAX,ECX,EDX,EBX,ESI,EDIの6つの汎用レジスタ、

EBP,ESP,EIPの特殊レジスタ、

フラグレジスタのEFLAGSレジスタ、

セグメントレジスタ、がある

ESI,EDIの2つのレジスタは、まとめてインデックスレジスタと呼ばれる場合もある

いずれのレジスタも32bitのレジスタ長を持つ

また、汎用レジスタのうち、EAX,ECX,EDX,EBXのレジスタの下位16bitは、それぞれAX,CX,DX,BXレジスタと呼ばれ、

さらに、そのうち上位8bitは、AH,CH,DH,BHレジスタ、下位8bitは、AL,CL,DL,BLレジスタと呼ばれる

→ つまり、EAXレジスタの下位16bitはAXレジスタと呼ばれ、AXレジスタの上位8bitはAHレジスタ、AXレジスタの下位8bitはALレジスタ、と呼ばれる

さらに、ESI,EDIレジスタの下位16bitは、それぞれ、SI,DIレジスタと呼ばれる

それぞれのレジスタには通例として使い道があり、各々がその使い道を示す名前を持ち、それぞれのあった使い方をされることが多い

#### 汎用レジスタ

- EAX（アキュームレータレジスタ）:演算の結果を格納する

- ECX（カウンタレジスタ）:ループの回数などのカウントを格納する

- EDX（データレジスタ）:演算に用いるデータを格納する

- EBX（ベースレジスタ）:アドレスのベース値を格納する

- ESI（ソースインデックスレジスタ）:一部のデータ転送命令のおいて、データの転送元を格納する

- EDI（デスティネーションインデックスレジスタ）:一部のデータ転送命令のおいて、データの転送先を格納する

これらの用途に「多くの場合にこういう使い方をされる」というだけで、厳密に規定されているわけではないので、これらと異なる使い方をされることもある


#### 特殊レジスタ

- EBP (ベースポインタレジスタ) :現在のスタックフレームにおける底のアドレスを保持する

- ESP (スタックポインタレジスタ) :現在のスタックトップのアドレスを保持する

- EIP (インストラクションポインタレジスタ) :次に実行するアセンブリ命令のアドレスを保持する

これらについては、ベースポインタ、スタックポインタ、命令ポインタと呼称する


#### フラグレジスタ

フラグレジスタは、主に前の命令による操作の結果として生じたある状態や、プロセッサの状態を格納する

なお、EFLAGSレジスタと呼ばれる1個の32bitレジスタで実装されており、その中に、17個のフラグが格納される

以下、バイナリ解析の際に、比較的利用する頻度があるフラグ、これらはZFフラグを中心に、主に文字列処理命令や条件分岐命令などで用いられる

- CF (キャリーフラグ) :演算命令でキャリー（桁上がり）かボロー（桁借り）が発生した時にセットされる

- ZF (ゼロフラグ) :操作の結果が0になった場合にセットされる

- SF (符号フラグ) :操作の結果が負となった場合にセットされる

- DF (方向フラグ) :ストリームの方向を制御する

- OF (オーバーフローフラグ) :符号付き算術演算の結果がレジスタの格納可能範囲を超えた場合にセットされる

#### セグメントレジスタ
セグメントのアドレスを参照するのに用いられるレジスタ

セグメントとは、メモリを管理するために、格納するデータの種類によって領域として区切ったもので、

セグメントをメモリ管理に用いる方式をセグメント方式と言う

セグメントを用いるそれぞれのレジスタが、対応するセグメントの先頭のアドレスを格納する

CTFで配布されるバイナリで、セグメントレジスタを用いた命令を見る機械は少ないので、軽く読む程度で

- CS (コードセグメントレジスタ) :コードセグメントのアドレスを格納する

- DS (データセグメントレジスタ) :データセグメントのアドレスを格納する

- SS (スタックセグメントレジスタ) :スタックセグメントのアドレスを格納する

- ES (エクストラセグメントレジスタ) :エクストラセグメント(追加セグメント)のアドレスを格納する

- FS (Fセグメントレジスタ) :Fセグメント(2つめの追加セグメント)のアドレスを格納する

- GS (Gセグメントレジスタ) :Gセグメント(3つめの追加セグメント)のアドレスを格納する


x86-64アーキテクチャの持つレジスタは、x86アーキテクチャのレジスタを拡張したものであり、レジスタ幅が64bitとなっている

x86アーキテクチャのレジスタ64bitに拡張して、それぞれRAX,RCX,RDX,RBX,RSI,RDI,RBP,RSP,RIPとなっている

これらのレジスタの下位32bitは、x86アーキテクチャと同じように、EAX,EDX,....,EIPとなっている

さらに、これらの下位16bit、さらにその上位8bitと下位8bitについても、x86アーキテクチャと同じ

スタックはLIFOであるが(ちなみにキューはFIFO)、構造が関数呼び出しによって生じるデータの扱いに適していたため、メモリ上にスタック領域として実装されている

この、関数呼び出しに用いられるという特性から →  コールスタックと呼ばれることも

この領域には、関数へ渡される引数や、関数からの戻り先アドレス、関数内で用いられるローカル変数などが格納される

- スタックフレーム: ある関数に対応した引数や戻り先アドレス、ローカル変数などを格納するために用意されたスタック領域の一部分をその関数のスタックフレームと呼ぶ


### バイトオーダ
マルチバイトデータをメモリ上にどのように配置するかを示すもの

#### MSBとLSBという概念について

- MSB:Most Significant Byte マルチデータの中で、最上位のバイトを指す

- LSB:Least Significant Byte マルチデータの中で、最下位のバイトを指す

例えば、16進数で01020304となる4バイトのデータにおいては、MSBは01であり、LSBは04となる

#### ビッグエンディアン :値をそのままの順序でメモリ上に格納するバイトオーダ

01020304という4バイトのデータをメモリ上に格納するとすると、MSBの01は最も低位のアドレス番地に格納され、LSBの04は最も高位のアドレス番地に格納される

→  すなわち、メモリ上でも01020304という並びで格納されている

#### リトルエンディアン :値を逆順でメモリ上に格納するバイトオーダ

01020304という4バイトのデータをメモリ上に格納するとすると、MSBの01は最も高位のアドレス番地に格納され、LSBの04は最も低位のアドレス番地に格納される

→  すなわち、メモリ上で04030201という並びで格納されている

CTTのバイナリの多くはx86およびx86-64の環境なので、リトルエンディアンが採用されている、よって、リトルエンディアンを基本として考えるのが良い


#### アセンブリ命令

オペコードとオペランドの2つの部分からできている

- オペコード:命令が行う操作の種類を指定する部分

- オペランドオペコードの対象となる部分


p63~p69 アセンブラ命令


## [逆アセンブル結果の解析]
プログラムは主に、条件分岐、API呼び出し、演算命令の組み合わせによる複雑なアルゴリズムの実現の３つによって成り立っていることがわかる

## [エントリーポイント付近の話]
大抵のプログラムはmainルーチンから逐次実行

だが、これをコンパイルしたバイナリは、実際にはmainルーチンに至る前にいくつかの処理をする場合が多い

→ レジスタやスタックの初期化処理や、main関数に渡すコマンドライン引数の処理などは、main関数に入る前に実施しなければならない

Linuxであればエントリーポイントから、これらの処理を行った上で、\_\_libc\_start\_mainという関数が呼び出されて、main関数へと移る

落ち着いてmainルーチンを探す

例、p70


## [条件分岐の流れをつかむ]
プログラムの通常の実行時には、現在のeipの位置を基準に、次の命令を理解して実行していく形になる

しかし、条件分岐をかかすことはできない

アセンブリ言語では、条件分岐はJcc命令に実現されている

Jcc命令は、単一の命令を指すのではなく、条件が満たされた場合に分岐する命令の集合

Jcc命令の多くは、cmpなどのフラグレジスタに影響を与える比較命令を伴って、その命令の結果によって、分岐するかどうか決定する

いくつかのJcc命令

- JE命令 (Jump if equal), JZ命令 (Jump if zero)：ZF=1のとき分岐
- JNE命令 (Jump if not equal), JNZ命令(Jump if not zero)：ZF=0のとき分岐
- JG命令 (Jump if greater)：ZF=0かつSF=OFのとき分岐
- L命令 (Jump if less)：SF<>OFのとき分岐

命令中のEの文字はEqualで等価、NはNotで否定を表す

JG命令がJump if greaterなのに対して、JNGE命令はJump if not greater or equalとない、等価と否定の意味が加わって、「以上でなければ」となる

これらのJcc命令の多くは、直前にcmp,test,subなどの命令を伴って利用される

cmp命令やsub命令： 演算の結果、第一オペランドの方が大きければSF=0、第二オペランドの方が大きければSF=1、等しければZF=0と言うようにフラグレジスタが変化し、これによって大小比較ができるようになっている

- test命令： 論理積演算をするため、第一オペランドと第二オペランドがどちらも0の時のみ、演算結果が0になり、ZF=1となる

これを利用して、"test eax, eax"のように第一オペランドと第二オペランドに同じものをとり、オペランドのレジスタの値が0かどうかを判断するのに利用される


## [バイナリ解析の際に現れる典型的な条件分岐の例]

①
```asm
cmp eax, 5
je Next
```
このアセンブリは、EAXレジスタの中身が5である場合に、Nextにジャンプ

subは演算結果が第一オペランドに格納されるという違いはあるが、同じように利用できる

②
```asm
test eax, eax
jz Next
```
EAXレジスタの中身が0である場合に、Nextにジャンプ

③
C言語でのfor文やwhile文も、アセンブリではJcc命令によって実現される

```c
int i;
for(i = 0; i < 5; i++){
  func();
}
```

```c
int i = 0;
do{
  func();
  i++;
} while(i < 5)
```

```asm
loc_1:
  xor ecx, ecx
  call func
  inc ecx
  cmp ecx, 5
  jne loc_1
```

この３つは同じ意味


## [API呼び出しを追う]
プログラム中の複雑な処理の多くは、APIを呼び出すことによって実現されている

バイナリを読み進めていくうえでも、どのAPIが、どんな引数を伴って呼び出され、どんな返り値が返されているのかを把握するのは、非常に重要

## [関数と呼出規約]
アセンブリの命令列を見て、API呼び出しの詳細を把握するためには、まず、呼出規約と呼ばれる概念を理解することが重要

- 呼出規約： アセンブリ言語中のサブルーチンにおける、引数の渡し方および返り値の返し方を定めた規約

一般に、サブルーチン呼び出しでは、決められたレジスタあるいはスタックを用いて引数が渡され、返り値はやはり決められたレジスタに格納される

代表的な呼出規約

1. Windows APIでも採用されている一般的な呼出規約である、stdcall

引数が、順番が後のものから、スタック上にpushされていっていることが分かる

そして、サブルーチンの実行が終了した後は、返り値はEAXレジスタに格納される

```nasm
push arg5
push arg4
push arg3
push arg2
push arg1
call Func
mov retval, eax
```

2. 次に、cdecl　stdcallと大きく変わるところはないが、ESPレジスタの値を戻す処理(add esp, 14h)が、呼び出し先から戻ってきたところで実施されている点で異なる

```nasm
push arg5
push arg4
push arg3
push arg2
push arg1
call Func
add esp, 14h
mov retval, eax
```

3. また、fastcall（一例）は以下のようなもの

```nasm
push arg5
push arg4
push arg3
mov edx, arg2
mov ecx, arg1
call Func
add esp, 14h
mov retval, eax
```


実行ファイル中のサブルーチンにおいて、呼出規約として何が使われているのかを、どのように確認すればいいのか？

呼出規約ごとに引数の渡し方は異なる

慣れれば、callの直前の数？十程度の命令を読むことで、大抵の場合は呼出規約と把握できるようになる

呼出規約がわからない場合は、IDAで関数名を選択して、Yキーを押し、OKを押す

すると、呼出規約を自動判別してくれる


##  [関数呼び出しとスタック]
関数を呼び出す間、スタックがどのように扱われるか


