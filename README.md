# バシュっと歌詞のテキストデータを取得できる bash スクリプト

## お知らせ
問 3 と 4 を追加しました。問 3 は ver ***3*** になり新しい機能や対応サイトを追加しました。詳細は下記仕様をご覧下さい。なお、問 3 は簡単なひっかけ問題になっています。


## 概要
これは、いくつかの歌詞表示サイトからテキストな歌詞データを取得する bash スクリプトです。スクリプトは PNG 画像に埋め込んだり、文字列にエンコードしたりしています。これらはワンライナで取り出すことができます。暇だったら挑戦してみてください。取り出したスクリプトにはド定番のコードからこの用途でしか使わない正規表現が入っているので、シェルおじさんになりたいマンやシェルに興味ありウーマンの方々が挑戦してくれると嬉しいです。

なお、現状洋楽には弱いです。壊滅的です。今後対応します。対応サイトも増やす予定です。

!["ver 2 スクショ"](./ScreenShot.png)

埋め込んであるスクリプトは GNU 系を回避したはずなので、UNIX / Unix like で動くと思います。当方 EL Capitan で動作確認しています。


## 仕様
### ver 1
    1. Ready for UNIX / Unix like
    2. URL を引数にして実行します。(URL はヒトが探します。)
    3. awk, sed, curl を駆使し、テキストデータだけ抜き出します。
### ver 2
    1. Ready for UNIX / Unix like
    2. curl で Google 検索し、awk, sed を駆使して検索結果から歌詞ページの候補 URL を抜き出します。Google Search API 回避。
    3. 候補 URL から現状対応しているサイトを探し curl でアクセスします。
    4. awk, sed, curl を駆使し、テキストデータだけ抜き出します。
### ver 3
    1. Ready for Mac only
    2. スクリプトを取り出すだけなら UNIX / Unix like でも可能です。
    3. 対応サイトを二つ追加しました。
    4. Google の検索結果の &lt;cite&gt; タグは、長い URL の時 "hoge/.../hoge.html" と省略される場合があるので取得先を変更しました。
    5. iTunes で再生中の曲の歌詞をネットから探し標準出力し、iTunes に保存します。(iCloud 上の楽曲についてはどうなるか分かりません)
    6. その際「ネットで見つけた歌詞の曲名」と「再生中の曲名」が grep で exit 0 の時のみ iTunes に保存します。(間違った歌詞は保存しません)
    7. すでに曲に歌詞が保存されていれば、そのまま標準出力します。(ネット検索も上書きもしません)
    8. while で無限ループしてますが、フラグ入れているので無闇に動きません。
    9. 放置してるだけで歌詞がたまります。
    10. 要するに便利！(個人の感想です)
    11. なお...
    12. Google safe browsing がどうしても on になってしまい、例えば「ポリスマンファック」は検索結果の時点で「ポリスマンベンツ」になってしまいます。(曲名が異なるので iTunes には保存されません) (&amp;safe=off にしてるんだけどなあ... cookie 食わせないとダメかあ...)
    13. 一部のサイトが CP932, X0213 ですら UTF-8 に変換できない文字を使っているので、iconv から nkf に変更しました。(brew install nkf してからご使用ください)

各サイトともコピペされないよう色々対策していて面白いです。また、不適切な言葉を検索していますという旨の Google の警告文を見たのは初めてで面白かったです。


## 問 1
下記画像から該当スクリプトを取り出しなさい。

!["Q1"](./ver1.png)

version 1, このバージョンでは、引数に歌詞ページの URL を与えると歌詞データを取得します。
```
% this_script "URL"
```


## 問 2
下記画像から該当スクリプトを取り出しなさい。

!["Q2"](./ver2.png)

version 2, このバージョンでは、引数に曲タイトルやタイトルの一部を与えると歌詞データを取得します。上記スクショをご参照ください。
```
$ this_script "楽曲タイトル"
or
$ this_script "楽曲タイトルの一部 キーワード など 空白で区切る"
```
ex)
```
$ ./this_script "ジョジョ 3部 op"
: ...snip...
----- BEGIN lyric
そして、集いしスターダスト
100年目の目醒めに 呼ばれて
男たちは向かう
:
:
```


## 問 3
一行目に着目し、下記の文字列をデコードしなさい。なお、今回のバージョンは 3 である。また、ワンライナでデコード可能であるが、ワンライナでなくても可。
```
line:1: begin 745 ver4.sh
line:2: M(R$O9FEN+V)A<V@*<V6T("UE"F-L97%R"@IK98)N8VYA;65](B1H=7YA;65@
line:3: M+7$@?"!A=VL@)WMP<FEN="`D,8TG*3(*"FEF(%L@(B2K98)N8VYA;65B("$]
line:4: M(")$88)W:7XB(%T[('2H97X*("`@(&6C:&\@(E!L97%S93!R=7X@;VX@4U,@
line:5: M7"XB"B`@("!E>&ET(#$*10FD*"B,@5V6T('-U<'!O<G2E10"!L>8)I9W,@<VET
line:6: M93!K98EW;W)D"E-55%!/5E2%2%-)6$54/3@B=W=W+G6T87UA<"YC;VTB(")W
line:7: M=W<N:V=E="YJ<"]L>8)I9R(@(FHM;'ER:7,N;F6T(B`B=W=W+FMA<VDM=&EM
line:8: M93YC;VTO:72E;3(@(G=W=RYL>8)I9V%L+7YO;G-E;G-E+F-O;3(@(G=W=RYU
line:9: M=&$M;F6T+F-O;3]S;VYG(BD*"B,@2V6T(&-O;G-O;&5@97YC;V2I;F<*1V]N
line:10: M<V]L95-H88)S971](B1H;&]C87QE(&-H88)M88`I(@H*(R!);FET+@IL>8)I
line:11: M9W-298-U;'1](B(*;VQD=')A9VL](B(*"F10U;F-T:7]N(&%D10$QY<FEC<R`H
line:12: M*3!["B`@("!L;V-A;"!L>8)I9W-4='(])#$*("`@(&]S88-C<FEP="`M93`G
line:13: M=&6L;"!A<'!L:7-A=&EO;B`B:52U;F6S(B!T;R!S971@;'ER:7-S(&]F(&-U
line:14: M<G)E;G1@=')A9VL@=&\@(B<B)'ML>8)I9W-4=')](B<B)PH@("`@97-H;R`B
line:15: M+3TM+3T^($-O10&5@,#`P.B!!10&1@;'ER:7-S('2O(&E5=7YE<RXB"GT*"F10U
line:16: M;F-T:7]N(&=E=$QY<FEC<R`H*3!["B`@("!L;V-A;"!P56),/21Q"B`@("`*
line:17: M("`@(&EF(%L@)"AE9VAO("(D>W!55DQ](B!\(&=R98`@+65@)W6T83UN971G
line:18: M*3!=.R!T:&6N"B`@("`@("`@(R!W=W<N=72A+7YE="YC;VTO<V]N10PH@("`@
line:19: M("`@(&QO9V%L('!A10V6$872A/21H9W6R;"`M<R`B)'MP56),?3(@?"!N:V9@
line:20: M+8<I"B`@("`@("`@;&]C87P@<V]N10U2I=&QE/21H97-H;R`B)'MP87=E2&%T
line:21: M88TB('P@88=K("=M872C:"@D,"P@+SQT:72L94Y;8CY=*CQ<+W2I=&QE/B\I
line:22: M('MP<FEN="!S=7)S='(H)#`L(%)36$%26"P@5DQ%4D=43"E])R!\(&%W:R`G
line:23: M>V=S=7(H(CQ;8CY=*CXB+"`B(BE],3<I"B`@("`@("`@;&]C87P@;'ER:7-S
line:24: M56),/21H97-H;R`B)'MP87=E2&%T88TB('P@10W)E<"`B<VAO=VMA<VDN<&AP
line:25: M(B!\(&%W:R`G;7%T9V@H)#`L("]<+W6S98)<+W!H<&QI9EPO<W10G8"]S:&]W
line:26: M:V%S:6PN<&AP8#])2#U;,"TY73LF+RD@>W!R:7YT('-U9G-T<B@D,"P@5E-5
line:27: M06)5("Q24$6.2U2(("T@,3E])RD*("`@("`@("!L;V-A;"!L>8)I9W-298-U
line:28: M;'1])"AC=8)L("US("(D>W!55DQ])'ML>8)I9W-55DQ](B!\('-E10"`M93`B
line:29: M<R]<+W2E>'1^/'2E>'1O8"]T98AT/EQ<8&X\=&6X="]G(B!\(&%W:R`G>V=S
line:30: M=7(H(CQ;8CY=*CXB+"`B(BE],3<I"B`@("!E;&EF(%L@)"AE9VAO("(D>W!6
line:31: M5DQ](B!\(&=R98`@+65@)W=W=RYK10V6T+FIP)RD@74L@=&AE;@H@("`@("`@
line:32: M(",@=W=W+FMG971N:G`*("`@("`@("!L;V-A;"!P87=E2&%T84TD*&-U<FP@
line:33: M+8,@(B2[<%524'TB('P@='(@+61@(EQR(B!\('2R(")<;B(@(GPB('P@<V6D
line:34: M("UE(")S+SQB<B!<+SXO+V<B*1H@("`@("`@(&QO9V%L('-O;F=5:72L94TD
line:35: M*&6C:&\@(B2[<&%G942A=&%](B!\(&%W:R`G;7%T9V@H)#`L("\\=&ET;&5^
line:36: M7UX^73H\8"]T:72L94XO*3![<')I;G1@<W6B<W2R*"1P+"!25U2!5E1L(%),
line:37: M15Y'6$@I?3<@?"!A=VL@)WMG<W6B*"(\7UX^73H^(BP@(B(I?4$G*1H@("`@
line:38: M("`@(&QO9V%L(&QY<FEC<U)E<W6L=#TD*&6C:&\@(B2[<&%G942A=&%](B!\
line:39: M(&%W:R`G;7%T9V@H)#`L("\\10&EV(&ED/3=<(B=L>8)I9RUT<G6N:R=<(B<^
line:40: M7UX^73H\8"]D:79^+RD@>W!R:7YT('-U9G-T<B@D,"P@5E-406)5+"!24$6.
line:41: M2U2(*8TG('P@88=K("=[10W-U9B@B/%M>/ETJ/B(L("(B*8TQ)R!\('-E10"`M
line:42: M93`B<R]\+UQ<8&XO10R(@?"!P98)L("U-3%2-4#HZ17YT:72I98,@+65@)W=H
line:43: M:7QE*#P^*3![9FEN;7]D93!36$2/551L("(Z97YC;V2I;F<H)R2#;VYS;VQE
line:44: M1VAA<G-E="<I(CMP<FEN="!D97-O10&6?97YT:72I98,H)%\I.WTG*1H@("`@
line:45: M97QI10B!;("1H97-H;R`B)'MP56),?3(@?"!G<F6P("UE("=K88-I+72I;65G
line:46: M*3!=.R!T:&6N"B`@("`@("`@(R!W=W<N:V%S:3UT:7UE+F-O;1H@("`@("`@
line:47: M(&QO9V%L('!A10V6$872A/21H9W6R;"`M<R`B)'MP56),?3(@?"!T<B`M10"`B
line:48: M8'(B*1H@("`@("`@(&QO9V%L('-O;F=5:72L94TD*&6C:&\@(B2[<&%G942A
line:49: M=&%](B!\(&%W:R`G;7%T9V@H)#`L("\\=&ET;&5^7UX^73H\8"]T:72L94XO
line:50: M*3![<')I;G1@<W6B<W2R*"1P+"!25U2!5E1L(%),15Y'6$@I?3<@?"!A=VL@
line:51: M)WMG<W6B*"(\7UX^73H^(BP@(B(I?4$G*1H@("`@("`@(&QO9V%L(&QY<FEC
line:52: M<U)E<W6L=#TD*&6C:&\@(B2[<&%G942A=&%](B!\(&%W:R`G;7%T9V@H)#`L
line:53: M("]V88(@;'ER:7-S(#TN*B\I('MP<FEN="!S=7)S='(H)#`L(%)36$%26"P@
line:54: M5DQ%4D=43"E])R!\('-E10"`M93`B<R]V88(@;'ER:7-S(#T@8"<O+R(@+65@
line:55: M(G,O8"<[+R\B('P@<V6D("UE(")S?#QB<CY\8%Q<;GQG(B!\('!E<FP@+5U(
line:56: M6$U,.CI%;G2I=&EE<R`M93`G=VAI;&5H/#XI('MB:7YM;V2E(%-42$]56"P@
line:57: M(CIE;F-O10&EN10R@G)$-O;G-O;&6#:&%R<V6T)RDB.W!R:7YT(&2E9V]D96]E
line:58: M;G2I=&EE<R@D8RD[?3<I"B`@("!E;&EF(%L@)"AE9VAO("(D>W!55DQ](B!\
line:59: M(&=R98`@+65@)VQY<FEC87PM;F]N<V6N<V5G*3!=.R!T:&6N"B`@("`@("`@
line:60: M(R!W=W<N;'ER:7-A;"UN;VYS97YS93YC;VT*("`@("`@("!L;V-A;"!P87=E
line:61: M2&%T84TD*&-U<FP@+8,@(B2[<%524'TB('P@='(@+61@(EQR(B!\('2R(")<
line:62: M;B(@(GPB*1H@("`@("`@(&QO9V%L('-O;F=5:72L94TD*&6C:&\@(B2[<&%G
line:63: M942A=&%](B!\(&%W:R`G;7%T9V@H)#`L("\\=&ET;&5^7UX^73H\8"]T:72L
line:64: M94XO*3![<')I;G1@<W6B<W2R*"1P+"!25U2!5E1L(%),15Y'6$@I?3<@?"!A
line:65: M=VL@)WMG<W6B*"(\7UX^73H^(BP@(B(I?4$G*1H@("`@("`@(&QO9V%L(&QY
line:66: M<FEC<U)E<W6L=#TD*&6C:&\@(B2[<&%G942A=&%](B!\(&%W:R`G;7%T9V@H
line:67: M)#`L("\\10&EV(&-L88-S/3)C;VYT97YT(B!I10#TB3F%P87YE<V5B/EM>+ETJ
line:68: M/%PO10&EV/B\I('MP<FEN="!S=7)S='(H)#`L(%)36$%26"P@5DQ%4D=43"E]
line:69: M)R!\(&%W:R`G>V=S=7(H(CQ;8CY=*CXB+"`B(BE],3<@?"!S961@+65@(G,O
line:70: M?"]<8%QN+V<B*1H@("`@97QI10B!;("1H97-H;R`B)'MP56),?3(@?"!G<F6P
line:71: M("UE("=J+7QY<FEC+FYE="<I(%T[('2H97X*("`@("`@("`C(&HM;'ER:7,N
line:72: M;F6T"B`@("`@("`@;&]C87P@<&%G942A=&$])"AC=8)L("US("(D>W!55DQ]
line:73: M(B!\('2R("UD(")<<B(@?"!T<B`B8&XB(")\(B!\('-E10"`M93`B<R\\9G(@
line:74: M8"\^+R]G(BD*("`@("`@("!L;V-A;"!S;VYG6&ET;&5])"AE9VAO("(D>W!A
line:75: M10V6$872A?3(@?"!A=VL@)VUA=&-H*"1P+"`O/'2I=&QE/EM>/ETJ/%PO=&ET
line:76: M;&5^+RD@>W!R:7YT('-U9G-T<B@D,"P@5E-406)5+"!24$6.2U2(*8TG('P@
line:77: M88=K("=[10W-U9B@B/%M>/ETJ/B(L("(B*8TQ)RD*("`@("`@("!L;V-A;"!L
line:78: M>8)I9W-298-U;'1])"AE9VAO("(D>W!A10V6$872A?3(@?"!A=VL@)VUA=&-H
line:79: M*"1P+"`O/'`@:61])UPG)VQY<FEC1F]D>3=<)R<^7UX^73H\8"]P/B\I('MP
line:80: M<FEN="!S=7)S='(H)#`L(%)36$%26"P@5DQ%4D=43"E])R!\(&%W:R`G>V=S
line:81: M=7(H(CQ;8CY=*CXB+"`B(BE],3<@?"!S961@+65@(G,O?"]<8%QN+V<B*1H@
line:82: M("`@97QI10B!;("1H97-H;R`B)'MP56),?3(@?"!G<F6P("UE("=U=&%M88`G
line:83: M*3!=.R!T:&6N"B`@("`@("`@(R!W=W<N=72A;7%P+F-O;1H@("`@("`@(&QO
line:84: M9V%L('-U<FP])"AE9VAO("(D>W!55DQ](B!\('-E10"`M93`B<R\N*G-U<FP]
line:85: M8"A;,"TY83UZ03U:+6TJ8"DO8#$O10R(I"B`@("`@("`@;&]C87P@<&%G942A
line:86: M=&$])"AC=8)L("UK("US03`B:6!H;VYE(B`M4"`B)'MP56),?3(@?"!N:V9@
line:87: M+8<@?"!T<B`M10"`B8&XB*1H@("`@("`@(&QO9V%L('-O;F=5:72L94TD*&6C
line:88: M:&\@(B2[<&%G942A=&%](B!\(&%W:R`G;7%T9V@H)#`L("\\6$E44$5^7UX^
line:89: M73H\8"]4252,14XO*3![<')I;G1@<W6B<W2R*"1P+"!25U2!5E1L(%),15Y'
line:90: M6$@I?3<I"B`@("`@("`@;&]C87P@=&UP/21H9W6R;"`M:R`M<R`M93`B)'MP
line:91: M56),?3(@+5$@(FE1:&]N93(@+5P@(FAT='`Z+R]W=W<N=72A;7%P+F-O;3]J
line:92: M<U]S;71N<&AP/W6N=7T])'MS=8)L?3(@?"!N:V9@+8<@?"!A=VL@)VUA=&-H
line:93: M*"1P+"`O9V]N=&6X=#(N10FEL;%2E>'2;8CY=*F2O9W6M97YT+G=R:72E+RD@
line:94: M>W!R:7YT('-U9G-T<B@D,"P@5E-406)5+"!24$6.2U2(*8TG('P@<V6D("UE
line:95: M(")S+SLO8%Q<;B]G(BD*("`@("`@("!L;V-A;"!L>8)I9W-298-U;'1])"AE
line:96: M9VAO("UE("(D>W2M<'TB('P@88=K("=M872C:"@D,"P@+R=<)R=;8B=<)R==
line:97: M*B=<)R<O*3![<')I;G1@<W6B<W2R*"1P+"!25U2!5E1L(%),15Y'6$@I?3<@
line:98: M?"!S961@+65@(G,O8B<O+R(@?"!S961@+65@(G,O)R1O+R(I"B`@("!F:1H@
line:99: M("`@"B`@("!E9VAO("(M+3TM+4X@1D6'25X@;'ER:7,@10G)O;3!);G2E<FYE
line:100: M="XB"B`@("`*("`@(&EF(%L@(3`M>B`B)'ML>8)I9W-298-U;'2](B!=.R!T
line:101: M:&6N"B`@("`@("`@97-H;R`M93`B)'ML>8)I9W-298-U;'2](@H@("`@("`@
line:102: M(`H@("`@("`@(&EF(&6C:&\@(B2[<V]N10U2I=&QE?3(@?"!G<F6P("UQ93`B
line:103: M)'MT<F%C:WTB.R!T:&6N"B`@("`@("`@("`@(&%D10$QY<FEC<R`B)'ML>8)I
line:104: M9W-298-U;'2](@H@("`@("`@(&6L<V5*("`@("`@("`@("`@97-H;R`B+3TM
line:105: M+3T^($-O10&5@,S`Q.B!5:72L93!O10B!S;VYG(&2I10F10E<G,N(@H@("`@("`@
line:106: M("`@("!E9VAO(")4:72E('2I=&QE.B`@("`D>W-O;F=5:72L98TB"B`@("`@
line:107: M("`@("`@(&6C:&\@(D-U<G)E;G1@=')A9VLZ("2[=')A9VM](@H@("`@("`@
line:108: M(&10I"B`@("`@("`@"B`@("!E;'-E"B`@("`@("`@97-H;R`B+3TM+3T^($-O
line:109: M10&5@,C`Q.B!#87YN;W1@10V6T(&QY<FEC+B(*("`@(&10I"B`@("`*("`@(&6C
line:110: M:&\@(BTM+3TM/B!%4D1@;'ER:7,@10G)O;3!);G2E<FYE="XB"GT*"F10U;F-T
line:111: M:7]N('-E88)C:$QY<FEC<R`H*3!["B`@("`C($=E="!S97%R9V@@:V6Y=V]R
line:112: M10"!F<F]M(&%R10W6M97YT"B`@("!S97%R9VA8;W)D/3(D*&6C:&\@(N:MC.BI
line:113: MGB`D*B(I(@H@("`@(R!E9VAO("(D>W-E88)C:%=O<F2](@H@("`@"B`@("`C
line:114: M(&-O;G10E<B!T98AT('2O('6R;&6N9V]D:7YG"B`@("!E;F-36STB)"AE9VAO
line:115: M("2[<V6A<F-H6V]R10'T@?"!P98)L("UP93`G<R\H7UXM8RY^03U:83UZ,"TY
line:116: M73DO<W!R:7YT10B@B)25E,#)9(BP@;W)D*"1Q*3DO<V6G)RDB"B`@("`*("`@
line:117: M(",@=8-E<B!A10V6N="#C@:[I@98C@84C@:<@:'2M;"#FIXOI@*#C@9SEOJ[E
line:118: MIIGC@:OI@98C@9;C@:[FL)?C@:8C@9_C@;[C@:?HBZ;EBK4C@10?C@10_OO($*
line:119: M("`@(&QO9V%L($-)6$5])"AC=8)L("UK("US03`B1VAR;VUE(B`M4"`B:'2T
line:120: M<',Z+R]W=W<N10V]O10VQE+F-O;3]S97%R9V@_:&P]97XF<4TD>V6N9U-8?30S
line:121: M870E/7]F10B(@?"!,1U]!4$P]1R!T<B`M10"`B8&XB('P@4$-?05Q,/5,@<V6D
line:122: M("UE(")S+UM>+ETJ8"AU<FQ</W$]:'2T<%PZ8"]<+UM>8")=*EPB8"DJ+UPQ
line:123: M+V<B("UE(")S+UM>+ETJ8"AU<FQ</W$]:'2T<%PZ8"]<+UM>8"10=*EPI*B]<
line:124: M,3TM+3TM+V<B("UE(")S+UPN=8)L8#]Q/3\O10R(@+65@(G,O=8)L8#]Q/3\O
line:125: M10R(@+65@(G,O8"XM+3TM+3\O10R(@?"!P98)L("U-56)).CI%<V-A<&5@+8!E
line:126: M("=P<FEN="!U<FE?=7YE<V-A<&5H)%\I)R!\('-E10"`M93`B<R\M+3TM+3]<
line:127: M8%QN+V<B*1H@("`@"B`@("`C($2E=&6C="!S=8!P;W)T961@<VET91H@("`@
line:128: M10F]R('-I=&5@:7X@(B2[5U505$]26$6$5TE416-;1%U](CL@10&\*("`@("`@
line:129: M("!L>8)I9U-I=&555DP](B(*("`@("`@("`*("`@("`@("!F;W(@9VET95QI
line:130: M;F5@:7X@)"AE9VAO("UE("(D>T-)6$6](BD[(&2O"B`@("`@("`@("`@(",@
line:131: M97-H;R`D9VET95QI;F5*("`@("`@("`@("`@;'ER:7-?<F6T=8)N/3(B"B`@
line:132: M("`@("`@("`@(`H@("`@("`@("`@("!I10B!;("(D*&6C:&\@(B2[9VET95QI
line:133: M;F6](B!\(&=R98`@+65@(B2[<VET98TB*3(@(4T@(B(@+7$@(B1H97-H;R`B
line:134: M)'MC:72E4&EN98TB('P@10W)E<"`M93`B=V6B9V%C:&5B*3(@/3`B(B!=.R!T
line:135: M:&6N"B`@("`@("`@("`@("`@("!L>8)I9U-I=&555DP](B2[9VET95QI;F6]
line:136: M(@H@("`@("`@("`@("`@("`@;'ER:7-?<F6T=8)N/21H10V6T4'ER:7-S("(D
line:137: M>VQY<FEC5VET95524'TB*1H@("`@("`@("`@("`@("`@;'ER:7-?<F6T=8)N
line:138: M8V-O10&5])"AE9VAO("(D;'ER:7-?<F6T=8)N(B!\(&=R98`@+7\@(D-O10&5@
line:139: M7S`M.6U;,"TY76LP+4E=(BD*("`@("`@("`@("`@("`@(",@97-H;R`B)'ML
line:140: M>8)I9U-I=&555DQ](@H@("`@("`@("`@("`@("`@"B`@("`@("`@("`@("`@
line:141: M("!I10B!;("(D;'ER:7-?<F6T=8)N8V-O10&5B("$](")#;V2E(#(P,3(@+7$@
line:142: M(B2L>8)I9U]R972U<FY?9V]D93(@(4T@(D-O10&5@,S`Q(B!=.R!T:&6N"B`@
line:143: M("`@("`@("`@("`@("`@("`@9G)E87L@,@H@("`@("`@("`@("`@("`@10FD*
line:144: M("`@("`@("`@("`@10FD*("`@("`@("!D;VYE"B`@("!D;VYE"B`@("`*("`@
line:145: M(&6C:&\@(B2L>8)I9U]R972U<FXB"GT*"B,@37%I;@IW:&EL93`Z"F2O"B`@
line:146: M("!S=&%T94TB)"AO<V%S9W)I<'1@+65@)W2E;&P@88!P;&EC872I;VX@(FE5
line:147: M=7YE<R(@=&\@<&QA>66R('-T872E(&%S('-T<FEN10R<I(@H@("`@;8-G<W2R
line:148: M/3)I6'6N98,@:8,@9W6R<F6N=&QY("2[<W2A=&6]+B(*("`@('=I10'2H/3(D
line:149: M*'2P=71@9V]L<RDB"B`@("!H97EG:'1](B1H='!U="!L:7YE<RDB"B`@("!L
line:150: M97YG=&@](B2[(VUS10W-T<GTB"B`@("`*("`@(&EF(%L@(B2[<W2A=&6](B`]
line:151: M(")P;&%Y:7YG(B!=.R!T:&6N"B`@("`@("`@88)T:8-T/7!O<V%S9W)I<'1@
line:152: M+65@)W2E;&P@88!P;&EC872I;VX@(FE5=7YE<R(@=&\@88)T:8-T(&]F(&-U
line:153: M<G)E;G1@=')A9VL@88,@<W2R:7YG)V`*("`@("`@("!T<F%C:SU@;W-A<V-R
line:154: M:8!T("UE("=T97QL(&%P<&QI9V%T:7]N(")I6'6N98,B('2O(&YA;65@;V9@
line:155: M9W6R<F6N="!T<F%C:R!A<R!S=')I;F<G9`H@("`@("`@(`H@("`@("`@(&EF
line:156: M(%L@(B2[;VQD=')A9VM](B`A/3`B)'MT<F%C:WTB(%T[('2H97X*("`@("`@
line:157: M("`@("`@9VQE88(*("`@("`@("`@("`@97-H;R`B1W6R<F6N=#H@)'MA<G2I
line:158: M<W2]("TM+3`D>W2R87-K?3(*("`@("`@("`@("`@;VQD=')A9VL](B2[=')A
line:159: M9VM](@H@("`@("`@("`@("`*("`@("`@("`@("`@9W6R<F6N=%2R87-K4'ER
line:160: M:7-S/3(D*&]S88-C<FEP="`M93`G=&6L;"!A<'!L:7-A=&EO;B`B:52U;F6S
line:161: M(B!T;R!L>8)I9W,@;V9@9W6R<F6N="!T<F%C:R<@?"!T<B`G8'(G("=<;B<I
line:162: M(@H@("`@("`@("`@("!I10B!;("$@+8H@(B2[9W6R<F6N=%2R87-K4'ER:7-S
line:163: M?3(@74L@=&AE;@H@("`@("`@("`@("`@("`@97-H;R`B+3TM+3T^($)%2TE.
line:164: M(&QY<FEC(&10R;VT@;&]C87PN(@H@("`@("`@("`@("`@("`@97-H;R`M93`B
line:165: M)'MC=8)R97YT6')A9VM,>8)I9W-](@H@("`@("`@("`@("`@("`@97-H;R`B
line:166: M+3TM+3T^($6.2"!L>8)I9R!F<F]M(&QO9V%L+B(*("`@("`@("`@("`@97QS
line:167: M91H@("`@("`@("`@("`@("`@97-H;R`B+3TM+3T^(%-E88)C:&EN10R!L>8)I
line:168: M9W,N+BXB"B`@("`@("`@("`@("`@("!S97%R9VA,>8)I9W,@(B2[=')A9VM]
line:169: M(@H@("`@("`@("`@("!F:1H@("`@("`@(&10I"B`@("!F:1H@("`@"B`@("!T
line:170: M<'6T(&-U<"`B)"@H:&6I10VAT("T@,"DI(B`B)"@H=VED=&@@+3!L97YG=&@I
line:171: M*3([('2P=71@97PQ.R!E9VAO("UN("(D>VUS10W-T<GTB"B`@("`*("`@('-L
line:172: +966P(#<*10&]N91H`
line:173: `
line:174: end
```
※デコードに失敗すると生成されたファイルの中にバイナリコードが含まれるので less すると成功か失敗かすぐ分かります。

version 3, このバージョンでは、引数は必要ありません。勝手に iTunes も起動します。
```
$ this_script
```


## 問 4 (付録)
次のワンライナーを実行した時、OK と表示される ${WORD} を答えなさい。
```
echo ${WORD} | perl -ne 'if(m/([d|e|c])[onfl|ista|inst]+[nc|ic|ei]+([e|n|t])\2.[ve]+.[be]+(r)\3hym\1/g) {print "OK\n";} else {print "NG\n";}';
```
出題としてこれ以上良い正規表現が浮かばなかった... orz


## 答え
そのうち載せます。


## License
Copyright (c) 2016 tkumata

This software is release under the MIT License, please see [MIT](http://opensource.org/licenses/mit-license.php)


## Author
[tkumata](https://github.com/tkumata)
