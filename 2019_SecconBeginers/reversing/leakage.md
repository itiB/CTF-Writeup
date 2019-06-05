# Leakage

友だちに教えてもらいながら解いたメモ

<https://score.beginners.seccon.jp/files/leakage_80a8c3c2bd63254a033ea21093944b1e.tar.gz>

## 解法

とりあえずまずは解凍する

`tar -zxvf leakage_80a8c3c2bd63254a033ea21093944b1e.tar.gz`

実行ファイルができるので実行してみる．

```bash
$ ./leakage
usage: ./leakage flag
$  ./leakage aaaa
wrong
```

どうやらaaaaのところにFlagを入れればいいみたい．
どんな感じのものなのか知りたいのでradareを使ってみるぞ．

radareじつは初挑戦

### radare2のインストール

`sudo apt install radare2`
で入れたけどなんかちょっとバージョンが古いので
`sudo apt remove radare2`した．

gitから持ってくるぞ `git clone git@github.com:radare/radare2.git`  
実行許可を与えて... `chmod +x sys/install.sh`  
インストール `sudo ./sys/install.sh`

### radare2を使っていく

```bash
$ radare2 ./leakage

# 初めにどのくらい解析するかを決めるらしい，aの数で決まるとのこと
# とりあえず一番ランクが上の aaaa

[0x00400500]> aaaa
[x] Analyze all flags starting with sym. and entry0 (aa)
[x] Analyze function calls (aac)
[x] Analyze len bytes of instructions for references (aar)
[x] Check for objc references
[x] Check for vtables
[x] Type matching analysis for all functions (aaft)
[x] Use -AA or aaaa to perform additional experimental analysis.
[x] Finding function preludes
[x] Enable constraint types analysis for variables
# まずはこの中で使われる関数一覧が見たい．
[0x00400500]>afl
0x00400500    1 42           entry0
0x00400540    4 42   -> 37   sym.deregister_tm_clones
0x00400570    4 58   -> 55   sym.register_tm_clones
0x004005b0    3 34   -> 29   entry.fini0
0x004005e0    1 7            entry.init0
0x00400c30    1 2            sym.__libc_csu_fini
0x004006d0    5 1258         sym.convert
0x004004e0    1 6            sym.imp.__stack_chk_fail
0x00400c34    1 9            sym._fini
0x00400bc0    3 101  -> 92   sym.__libc_csu_init
0x00400498    3 23           sym._init
0x004005e7    9 121          sym.is_correct
0x00400530    1 2            sym._dl_relocate_static_pie
0x00400660    7 111          main
0x004004c0    1 6            sym.imp.puts
0x004004d0    1 6            sym.imp.strlen
0x004004f0    1 6            sym.imp.printf
# mainがあるのでmainにターゲットを絞って可視化！！！
[0x00400500]> s main
[0x00400660]> VV
```

一度でいいからやってみたかったやつ

[![Image from Gyazo](https://i.gyazo.com/debc0535ff9302c1d9f0f5e4058f0568.png)](https://gyazo.com/debc0535ff9302c1d9f0f5e4058f0568)

解析していくぞい．
まずは一番上のところから

[![Image from Gyazo](https://i.gyazo.com/1008d32aabfda09f9ad5f60205d249be.png)](https://gyazo.com/1008d32aabfda09f9ad5f60205d249be)

`mov dword [var_4h], edi`，
`cmp dword [var_4h], 1` から第一引数(edi)が1ならばtrueに行くみたいですね．  
とりあえず引数としてFlagをいれなければ落ちることはわかっているのでtrueに行くほうだけを解析していきましょう

### 引数データの対応レジスタ

| レジスタ | 引数 |
| -- | -- |
| edi | 第一引数 |
| esi | 第二引数 |
| edx | 第三引数 |
| ecx | 第四引数 |
| r8d | 第五引数 |
| r9d | 第六引数 |

sys.is_callに入力を渡して呼び出す

[![Image from Gyazo](https://i.gyazo.com/c72703649e801281cb5b8aaa8621a5a1.png)](https://gyazo.com/c72703649e801281cb5b8aaa8621a5a1)

じゃあsys.is_correctみたいよね

`qq`でradare2のビジュアルモードを閉じる

```radare
[0x00400660]> s sym.is_correct
[0x004005e7]> VV
```

[![Image from Gyazo](https://i.gyazo.com/f494cc883e0e910cc9ab61277595d86d.png)](https://gyazo.com/f494cc883e0e910cc9ab61277595d86d)

注目すべきは最後らへんにあるcall  
`call sym.imp.strlen`の結果をraxにいれて34(0x22)と比較している．  
16進数を10進数に直して表記してくれてるの助かる

つまり34文字じゃなければはじくと...

trueの時をたどっていく，まぁあんまりここは気にしなくてよさそうとの話

[![Image from Gyazo](https://i.gyazo.com/54f989f9f850b133365e25ec96a51a78.png)](https://gyazo.com/54f989f9f850b133365e25ec96a51a78)

[![Image from Gyazo](https://i.gyazo.com/8d2480463d976758bb1c38d26d860e31.png)](https://gyazo.com/8d2480463d976758bb1c38d26d860e31)

ここですよ，なんか`enc_flag`とかいうのとcmpしている気配を感じる．  
`call sym.convert`で`enc_flag`を復号して`cmp byte [var_5h], al`で比較していそう．

Trueでたどりつく `0x40064f [oh]`で`add dword [var_4h], 1 `していることから  
1文字ずつずらして比較しているっぽい．

比較している値の入っているとこ見ればFlagわかるんじゃない？？

こっからはgdb-pedaの出番

### gdb-pedaをつかっていく

まずはsys.convertで復号された値を見るためにcallのあとにブレークポイントを入れたい．

```bash
$ gdb ./leakage
gdb-peda$ disas is_correct
Dump of assembler code for function is_correct:
   0x00000000004005e7 <+0>:	push   rbp
   ~~~~~~
   0x0000000000400622 <+59>:	movzx  eax,BYTE PTR [rdx+rax*1]
   0x0000000000400626 <+63>:	movzx  eax,al
   0x0000000000400629 <+66>:	mov    edi,eax
   0x000000000040062b <+68>:	call   0x4006d0 <convert>
   0x0000000000400630 <+73>:	mov    BYTE PTR [rbp-0x5],al
   0x0000000000400633 <+76>:	mov    eax,DWORD PTR [rbp-0x4]
   ~~~~~~
   0x000000000040065f <+120>:	ret    
End of assembler dump.
gdb-peda$ b *0x400630
Breakpoint 1 at 0x400630
```

よっしゃ，34文字じゃなきゃここまでたどり着けないから34文字いれてrunするで！

```gdb
gdb-peda$ r 1234567890123456789012345678901234
Starting program: /leakage 1234567890123456789012345678901234

[----------------------------------registers-----------------------------------]
RAX: 0x63 ('c')
RBX: 0x0 
RCX: 0x0 
```

RAXに入っているのがcmpで使われている値だ...!! 一つ目は`c`っと...

続きに進もうとすると間違ってるよって怒られる．

```gdb
gdb-peda$ c
Continuing.
wrong
[Inferior 1 (process 9784) exited normally]
Warning: not running
```

まぁせやな，じゃあ一文字目を`c`にして進めてみるか

```gdb
gdb-peda$ r c234567890123456789012345678901234
Starting program: /leakage c234567890123456789012345678901234

[----------------------------------registers-----------------------------------]
RAX: 0x63 ('c')
RBX: 0x0 
RCX: 0x0 
~~~~~~
一回目のときの
~~~~~~

gdb-peda$ c
Continuing.

[----------------------------------registers-----------------------------------]
RAX: 0x74 ('t')
RBX: 0x0 
RCX: 0x0 
```

つぎはcしても次には行けた...!!!2番目の文字は`t`  
なるほどな，これは一文字ずつ当てていけばいいわけだ．

```gdb
gdb-peda$ r ctf4b{le4k1ng_th3_f1ag_0ne_by_0ne}
~~~~~~~
~~~~~~~
gdb-peda$
Continuing.
correct
[Inferior 1 (process 9809) exited normally]
```

勝った...!!!!

## FLAG

ctf4b{le4k1ng_th3_f1ag_0ne_by_0ne}