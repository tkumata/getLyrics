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
line:2: M(R$O9FEN+V)A<V@*<V6T("UE"F-L97%R"@HC(%-E="!S=8!P;W)T961@;'ER
line:3: M:7-S('-I=&5@:V6Y=V]R10`HC5U505$]26$6$5TE416,]*")W=W<N=72A;7%P
line:4: M+F-O;3(@(G=W=RYK10V6T+FIP+VQY<FEC(B`B:BUL>8)I9RYN971B(")W=W<N
line:5: M:V%S:3UT:7UE+F-O;3]I=&6M(B`B=W=W+FQY<FEC87PM;F]N<V6N<V5N9V]M
line:6: M(B`B=W=W+G6T83UN971N9V]M+W-O;F<B*1I356!04U)41413252%5STH(G=W
line:7: M=RYU=&%M88`N9V]M(B`B=W=W+FMA<VDM=&EM93YC;VTO:72E;3(@(FHM;'ER
line:8: M:7,N;F6T(B`B=W=W+FQY<FEC87PM;F]N<V6N<V5N9V]M(B`B=W=W+G6T83UN
line:9: M971N9V]M+W-O;F<B*1H*(R!'971@9V]N<V]L93!E;F-O10&EN10PI#;VYS;VQE
line:10: M1VAA<G-E=#TB)"AL;V-A;&5@9VAA<FUA<"DB"@IL>8)I9W-298-U;'1](B(*
line:11: M;VQD=')A9VL](B(*"F10U;F-T:7]N(&%D10$QY<FEC<R`H*3!["B`@("!L;V-A
line:12: M;"!L>8)I9W-4='(])#$*("`@(&]S88-C<FEP="`M93`G=&6L;"!A<'!L:7-A
line:13: M=&EO;B`B:52U;F6S(B!T;R!S971@;'ER:7-S(&]F(&-U<G)E;G1@=')A9VL@
line:14: M=&\@(B<B)'ML>8)I9W-4=')](B<B)PH@("`@97-H;R`B+3T^($%D10"!L>8)I
line:15: M9W,@=&\@:52U;F6S+B(*?1H*10G6N9W2I;VX@10V6T4'ER:7-S("@I('L*("`@
line:16: M(&QO9V%L('!55DP])#$*("`@(`H@("`@:69@7R`D*&6C:&\@(B2[<%524'TB
line:17: M('P@10W)E<"`M93`G=72A+7YE="<I(%T*("`@('2H97X*("`@("`@("`C('=W
line:18: M=RYU=&$M;F6T+F-O;3]S;VYG"B`@("`@("`@;&]C87P@<&%G942A=&$])"AC
line:19: M=8)L("US("(D>W!55DQ](B!\(&YK10B`M=RD*("`@("`@("!L;V-A;"!S;VYG
line:20: M6&ET;&5])"AE9VAO("(D>W!A10V6$872A?3(@?"!A=VL@)VUA=&-H*"1P+"`O
line:21: M/'2I=&QE/EM>/ETJ/%PO=&ET;&5^+RD@>W!R:7YT('-U9G-T<B@D,"P@5E-5
line:22: M06)5+"!24$6.2U2(*8TG('P@88=K("=[10W-U9B@B/%M>/ETJ/B(L("(B*8TQ
line:23: M)RD*("`@("`@("!L;V-A;"!L>8)I9W-55DP])"AE9VAO("(D>W!A10V6$872A
line:24: M?3(@?"!G<F6P(")S:&]W:V%S:3YP:'`B('P@88=K("=M872C:"@D,"P@+UPO
line:25: M=8-E<EPO<&AP;&EB8"]S=F=<+W-H;W=K88-I8"YP:'!</TE$/6LP+4E=*R9O
line:26: M*3![<')I;G1@<W6B<W2R*"1P+"!25U2!5E1@+%),15Y'6$@@+3`Q*8TG*1H@
line:27: M("`@("`@(&QO9V%L(&QY<FEC<U)E<W6L=#TD*&-U<FP@+8,@(B2[<%524'TD
line:28: M>VQY<FEC<U524'TB('P@<V6D("UE(")S+UPO=&6X=#X\=&6X="]<+W2E>'1^
line:29: M8%Q<;CQT98AT+V<B('P@88=K("=[10W-U9B@B/%M>/ETJ/B(L("(B*8TQ)RD*
line:30: M("`@(&6L:69@7R`D*&6C:&\@(B2[<%524'TB('P@10W)E<"`M93`G=W=W+FMG
line:31: M971N:G`G*3!="B`@("!T:&6N"B`@("`@("`@(R!W=W<N:V=E="YJ<`H@("`@
line:32: M("`@(&QO9V%L('!A10V6$872A/21H9W6R;"`M<R`B)'MP56),?3(@?"!T<B`M
line:33: M10"`B8'(B('P@='(@(EQN(B`B?"(@?"!S961@+65@(G,O/&)R(%PO/B\O10R(I
line:34: M"B`@("`@("`@;&]C87P@<V]N10U2I=&QE/21H97-H;R`B)'MP87=E2&%T88TB
line:35: M('P@88=K("=M872C:"@D,"P@+SQT:72L94Y;8CY=*CQ<+W2I=&QE/B\I('MP
line:36: M<FEN="!S=7)S='(H)#`L(%)36$%26"P@5DQ%4D=43"E])R!\(&%W:R`G>V=S
line:37: M=7(H(CQ;8CY=*CXB+"`B(BE],3<I"B`@("`@("`@;&]C87P@;'ER:7-S5F6S
line:38: M=7QT/21H97-H;R`B)'MP87=E2&%T88TB('P@88=K("=M872C:"@D,"P@+SQD
line:39: M:79@:61])UPB)VQY<FEC+72R=7YK)UPB)SY;8CY=*CQ<+V2I=CXO*3![<')I
line:40: M;G1@<W6B<W2R*"1P+"!25U2!5E1L(%),15Y'6$@I?3<@?"!A=VL@)WMG<W6B
line:41: M*"(\7UX^73H^(BP@(B(I?4$G('P@<V6D("UE(")S+WPO8%Q<;B]G(B!\('!E
line:42: M<FP@+5U(6$U,.CI%;G2I=&EE<R`M93`G=VAI;&5H/#XI('MB:7YM;V2E(%-5
line:43: M2$]56"P@(CIE;F-O10&EN10R@G)$-O;G-O;&6#:&%R<V6T)RDB.W!R:7YT(&2E
line:44: M9V]D96]E;G2I=&EE<R@D8RD[?3<I"B`@("!E;&EF(%L@)"AE9VAO("(D>W!6
line:45: M5DQ](B!\(&=R98`@+65@)VMA<VDM=&EM93<I(%T*("`@('2H97X*("`@("`@
line:46: M("`C('=W=RYK88-I+72I;65N9V]M"B`@("`@("`@;&]C87P@<&%G942A=&$]
line:47: M)"AC=8)L("US("(D>W!55DQ](B!\('2R("UD(")<<B(I"B`@("`@("`@;&]C
line:48: M87P@<V]N10U2I=&QE/21H97-H;R`B)'MP87=E2&%T88TB('P@88=K("=M872C
line:49: M:"@D,"P@+SQT:72L94Y;8CY=*CQ<+W2I=&QE/B\I('MP<FEN="!S=7)S='(H
line:50: M)#`L(%)36$%26"P@5DQ%4D=43"E])R!\(&%W:R`G>V=S=7(H(CQ;8CY=*CXB
line:51: M+"`B(BE],3<I"B`@("`@("`@;&]C87P@;'ER:7-S5F6S=7QT/21H97-H;R`B
line:52: M)'MP87=E2&%T88TB('P@88=K("=M872C:"@D,"P@+W10A<B!L>8)I9W,@/3XJ
line:53: M+RD@>W!R:7YT('-U9G-T<B@D,"P@5E-406)5+"!24$6.2U2(*8TG('P@<V6D
line:54: M("UE(")S+W10A<B!L>8)I9W,@/3!<)R\O(B`M93`B<R]<)SLO+R(@?"!S961@
line:55: M+65@(G-\/&)R/GQ<8%QN?&<B('P@<&6R;"`M35A435PZ.D6N=&ET:66S("UE
line:56: M("=W:&EL93@\/BD@>V)I;FUO10&5@5U2$4U55+"`B.F6N9V]D:7YG*"<D1V]N
line:57: M<V]L95-H88)S971G*3([<')I;G1@10&6C;V2E8V6N=&ET:66S*"2?*4M])RD*
line:58: M("`@(&6L:69@7R`D*&6C:&\@(B2[<%524'TB('P@10W)E<"`M93`G;'ER:7-A
line:59: M;"UN;VYS97YS93<I(%T*("`@('2H97X*("`@("`@("`C('=W=RYL>8)I9V%L
line:60: M+7YO;G-E;G-E+F-O;1H@("`@("`@(&QO9V%L('!A10V6$872A/21H9W6R;"`M
line:61: M<R`B)'MP56),?3(@?"!T<B`M10"`B8'(B('P@='(@(EQN(B`B?"(I"B`@("`@
line:62: M("`@;&]C87P@<V]N10U2I=&QE/21H97-H;R`B)'MP87=E2&%T88TB('P@88=K
line:63: M("=M872C:"@D,"P@+SQT:72L94Y;8CY=*CQ<+W2I=&QE/B\I('MP<FEN="!S
line:64: M=7)S='(H)#`L(%)36$%26"P@5DQ%4D=43"E])R!\(&%W:R`G>V=S=7(H(CQ;
line:65: M8CY=*CXB+"`B(BE],3<I"B`@("`@("`@;&]C87P@;'ER:7-S5F6S=7QT/21H
line:66: M97-H;R`B)'MP87=E2&%T88TB('P@88=K("=M872C:"@D,"P@+SQD:79@9VQA
line:67: M<W,](F-O;G2E;G1B(&ED/3)*88!A;F6S93(^7UXN73H\8"]D:79^+RD@>W!R
line:68: M:7YT('-U9G-T<B@D,"P@5E-406)5+"!24$6.2U2(*8TG('P@88=K("=[10W-U
line:69: M9B@B/%M>/ETJ/B(L("(B*8TQ)R!\('-E10"`M93`B<R]\+UQ<8&XO10R(I"B`@
line:70: M("!E;&EF(%L@)"AE9VAO("(D>W!55DQ](B!\(&=R98`@+65@)VHM;'ER:7,N
line:71: M;F6T)RD@71H@("`@=&AE;@H@("`@("`@(",@:BUL>8)I9RYN971*("`@("`@
line:72: M("!L;V-A;"!P87=E2&%T84TD*&-U<FP@+8,@(B2[<%524'TB('P@='(@+61@
line:73: M(EQR(B!\('2R(")<;B(@(GPB('P@<V6D("UE(")S+SQB<B!<+SXO+V<B*1H@
line:74: M("`@("`@(&QO9V%L('-O;F=5:72L94TD*&6C:&\@(B2[<&%G942A=&%](B!\
line:75: M(&%W:R`G;7%T9V@H)#`L("\\=&ET;&5^7UX^73H\8"]T:72L94XO*3![<')I
line:76: M;G1@<W6B<W2R*"1P+"!25U2!5E1L(%),15Y'6$@I?3<@?"!A=VL@)WMG<W6B
line:77: M*"(\7UX^73H^(BP@(B(I?4$G*1H@("`@("`@(&QO9V%L(&QY<FEC<U)E<W6L
line:78: M=#TD*&6C:&\@(B2[<&%G942A=&%](B!\(&%W:R`G;7%T9V@H)#`L("\\<"!I
line:79: M10#TG8"<G;'ER:7-";V2Y)UPG)SY;8CY=*CQ<+W`^+RD@>W!R:7YT('-U9G-T
line:80: M<B@D,"P@5E-406)5+"!24$6.2U2(*8TG('P@88=K("=[10W-U9B@B/%M>/ETJ
line:81: M/B(L("(B*8TQ)R!\('-E10"`M93`B<R]\+UQ<8&XO10R(I"B`@("!E;&EF(%L@
line:82: M)"AE9VAO("(D>W!55DQ](B!\(&=R98`@+65@)W6T87UA<"<I(%T*("`@('2H
line:83: M97X*("`@("`@("`C('=W=RYU=&%M88`N9V]M"B`@("`@("`@;&]C87P@<W6R
line:84: M;#TD*&6C:&\@(B2[<%524'TB('P@<V6D("UE(")S+RXJ<W6R;#U<*%LP+4EA
line:85: M+8I!+6HM73I<*3]<,3]G(BD*("`@("`@("!L;V-A;"!P87=E2&%T84TD*&-U
line:86: M<FP@+7L@+8-!(")I5&AO;F5B("U,("(D>W!55DQ](B!\(&YK10B`M=R!\('2R
line:87: M("UD(")<;B(I"B`@("`@("`@;&]C87P@<V]N10U2I=&QE/21H97-H;R`B)'MP
line:88: M87=E2&%T88TB('P@88=K("=M872C:"@D,"P@+SQ4252,14Y;8CY=*CQ<+U2)
line:89: M6$Q%/B\I('MP<FEN="!S=7)S='(H)#`L(%)36$%26"P@5DQ%4D=43"E])RD*
line:90: M("`@("`@("!L;V-A;"!T;8`])"AC=8)L("UK("US("UE("(D>W!55DQ](B`M
line:91: M03`B:6!H;VYE(B`M4"`B:'2T<#HO+W=W=RYU=&%M88`N9V]M+VIS8W-M="YP
line:92: M:'`_=7YU;4TD>W-U<FQ](B!\(&YK10B`M=R!\(&%W:R`G;7%T9V@H)#`L("]C
line:93: M;VYT98AT,BYF:7QL6&6X=%M>/ETJ10&]C=7UE;G1N=W)I=&5O*3![<')I;G1@
line:94: M<W6B<W2R*"1P+"!25U2!5E1L(%),15Y'6$@I?3<@?"!S961@+65@(G,O.R]<
line:95: M8%QN+V<B*1H@("`@("`@(&QO9V%L(&QY<FEC<U)E<W6L=#TD*&6C:&\@+65@
line:96: M(B2[=&UP?3(@?"!A=VL@)VUA=&-H*"1P+"`O)UPG)UM>)UPG)UTJ)UPG)R\I
line:97: M('MP<FEN="!S=7)S='(H)#`L(%)36$%26"P@5DQ%4D=43"E])R!\('-E10"`M
line:98: M93`B<R]>)R\O(B!\('-E10"`M93`B<R\G)"\O(BD*("`@(&6L<V5*("`@("`@
line:99: M("`Z"B`@("!F:1H@("`@"B`@("!E9VAO("(M+4X@1D6'25X@;'ER:7,@10G)O
line:100: M;3!);G2E<FYE="XB.PH@("`@:69@7R`A("UZ("(D>VQY<FEC<U)E<W6L='TB
line:101: M(%T[('2H97X*("`@("`@("!E9VAO("UE("(D>VQY<FEC<U)E<W6L='TB.PH@
line:102: M("`@("`@(`H@("`@("`@(&EF(&6C:&\@(B2[<V]N10U2I=&QE?3(@?"!G<F6P
line:103: M("UQ93`B)'MT<F%C:WTB.R!T:&6N"B`@("`@("`@("`@(&%D10$QY<FEC<R`B
line:104: M)'ML>8)I9W-298-U;'2](CL*("`@("`@("!E;'-E"B`@("`@("`@("`@(&6C
line:105: M:&\@(BTM/B!#;V2E(#,P,4H@6&ET;&5@;V9@<V]N10R!D:70F98)S+B(["B`@
line:106: M("`@("`@("`@(&6C:&\@(E-I=&5@=&ET;&5Z("`@("2[<V]N10U2I=&QE?3([
line:107: M"B`@("`@("`@("`@(&6C:&\@(D-U<G)E;G1@=')A9VLZ("2[=')A9VM](CL*
line:108: M("`@("`@("!F:1H@("`@("`@(`H@("`@97QS91H@("`@("`@(&6C:&\@(BTM
line:109: M/B!#;V2E(#(P,4H@1V%N;F]T(&=E="!L>8)I9RXB.PH@("`@10FD*("`@(&6C
line:110: M:&\@(BTM/B!%4D1@;'ER:7,@10G)O;3!);G2E<FYE="XB.PI]"@IF=7YC=&EO
line:111: M;B!S97%R9VA,>8)I9W,@*"D@>PH@("`@(R!'971@<V6A<F-H(&ME>8=O<F1@
line:112: M10G)O;3!A<F=U;66N=`H@("`@<V6A<F-H6V]R10#TD*&6C:&\@(N:MC.BIGB`D
line:113: M*B(I"@H@("`@(R!C;VYV98(@=&6X="!T;R!U<FQE;F-O10&EN10PH@("`@97YC
line:114: M5U<])"AE9VAO("(D>W-E88)C:%=O<F2](B!\('!E<FP@+8!E("=S+RA;8BU?
line:115: M+GY!+6IA+8HP+4E=*3]S<')I;G2F*"(E)25P,E@B+"!O<F1H)#$I*3]S97<G
line:116: M*1H*("`@(",@2V]O10VQE('-E88)C:`H@("`@(R!#252%/21H9W6R;"`M:R`M
line:117: M<T$@(D-H<F]M93(@+5P@(FAT='!S.B\O=W=W+F=O;V=L93YC;VTO<V6A<F-H
line:118: M/VAL/66N)G$])'ME;F-36WTF<V%F94UO10F9B('P@4$-?1U195$5]1R!T<B`M
line:119: M10"`B8&XB('P@<V6D("UE(")S+SQB/B\O10R(@+65@(G,O/%PO9CXO+V<B("UE
line:120: M(")S+UM>+ETJ8"@\9VET94Y;8CY=*CQ<+V-I=&5^8"DJ+UPQ+V<B("UE(")S
line:121: M+UPN8'LR+%Q]+R]G(B`M93`B<R\\8"]C:72E/B]<8%QN+V<B('P@88=K("=[
line:122: M10W-U9B@B/%M>/ETJ/B(L("(B*8TQ)R!\('!E<FP@+5U(6$U,.CI%;G2I=&EE
line:123: M<R`M93`G=VAI;&5H/#XI('MB:7YM;V2E(%-42$]56"PB.F6N9V]D:7YG*"<D
line:124: M1V]N<V]L95-H88)S971G*3([<')I;G1@10&6C;V2E8V6N=&ET:66S*"2?*4M]
line:125: M)RD["B`@("`*("`@(",@=8-E<B!A10V6N="#C@:[I@98C@84C@:<@:'2M;"#F
line:126: MIXOI@*#C@9SEOJ[EIIGC@:OI@98C@9;C@:[FL)?C@:8C@9_C@;[C@:?HBZ;E
line:127: MBK4C@10?C@10_OO($*("`@(&QO9V%L($-)6$5])"AC=8)L("UK("US03`B1VAR
line:128: M;VUE(B`M4"`B:'2T<',Z+R]W=W<N10V]O10VQE+F-O;3]S97%R9V@_:&P]97XF
line:129: M<4TD>V6N9U-8?30S870E/7]F10B(@?"!,1U]!4$P]1R!T<B`M10"`B8&XB('P@
line:130: M4$-?05Q,/5,@<V6D("UE(")S+UM>+ETJ8"AU<FQ</W$]:'2T<%PZ8"]<+UM>
line:131: M8")=*EPB8"DJ+UPQ+V<B("UE(")S+UM>+ETJ8"AU<FQ</W$]:'2T<%PZ8"]<
line:132: M+UM>8"10=*EPI*B]<,3TM+3TM+V<B("UE(")S+UPN=8)L8#]Q/3\O10R(@+65@
line:133: M(G,O=8)L8#]Q/3\O10R(@+65@(G,O8"XM+3TM+3\O10R(@?"!P98)L("U-56))
line:134: M.CI%<V-A<&5@+8!E("=P<FEN="!U<FE?=7YE<V-A<&5H)%\I)R!\('-E10"`M
line:135: M93`B<R\M+3TM+3]<8%QN+V<B*1H@("`@"B`@("`C($2E=&6C="!S=8!P;W)T
line:136: M961@<VET91H@("`@10F]R('-I=&5@:7X@(B2[5U505$]26$6$5TE416-;1%U]
line:137: M(CL@10&\*("`@("`@("!L>8)I9U-I=&555DP](B(*("`@("`@("!F;W(@9VET
line:138: M95QI;F5@:7X@)"AE9VAO("UE("(D>T-)6$6](BD[(&2O"B`@("`@("`@("`@
line:139: M(",@97-H;R`D9VET95QI;F5*("`@("`@("`@("`@:69@7R`D*&6C:&\@(B2[
line:140: M9VET95QI;F6](B!\(&=R98`@+65@(B2[<VET98TB*3!=.R!T:&6N"B`@("`@
line:141: M("`@("`@("`@("!L>8)I9U-I=&555DP](B2[9VET95QI;F6](@H@("`@("`@
line:142: M("`@("`@("`@9G)E87L@,@H@("`@("`@("`@("!F:1H@("`@("`@(&2O;F5*
line:143: M("`@(&2O;F5*"B`@("!I10B!;("$@+8H@(B2[;'ER:7-4:72E56),?3(@74L@
line:144: M=&AE;@H@("`@("`@(&=E=$QY<FEC<R`B)'ML>8)I9U-I=&555DQ](@H@("`@
line:145: M("`@(&6C:&\@(BTM/B!(:71@)&QY<FEC5VET95524"(*("`@("!E;'-E"B`@
line:146: M("`@("`@97-H;R`B+3T^($-O10&5@,4`Q.B!397%R9VAE10"!S:72E<R!N;W1@
line:147: M<W6P<&]R=&6D+B(*("`@(&10I"GT*"B,@37%I;@IW:&EL93`Z"F2O"B`@("!S
line:148: M=&%T94TB)"AO<V%S9W)I<'1@+65@)W2E;&P@88!P;&EC872I;VX@(FE5=7YE
line:149: M<R(@=&\@<&QA>66R('-T872E(&%S('-T<FEN10R<I(@H@("`@;8-G<W2R/3)I
line:150: M6'6N98,@:8,@9W6R<F6N=&QY("2[<W2A=&6]+B(*("`@('=I10'2H/3(D*'2P
line:151: M=71@9V]L<RDB"B`@("!H97EG:'1](B1H='!U="!L:7YE<RDB"B`@("!L97YG
line:152: M=&@](B2[(VUS10W-T<GTB"B`@("`*("`@(&EF(%L@(B2[<W2A=&6](B`](")P
line:153: M;&%Y:7YG(B!=.R!T:&6N"B`@("`@("`@88)T:8-T/7!O<V%S9W)I<'1@+65@
line:154: M)W2E;&P@88!P;&EC872I;VX@(FE5=7YE<R(@=&\@88)T:8-T(&]F(&-U<G)E
line:155: M;G1@=')A9VL@88,@<W2R:7YG)V`*("`@("`@("!T<F%C:SU@;W-A<V-R:8!T
line:156: M("UE("=T97QL(&%P<&QI9V%T:7]N(")I6'6N98,B('2O(&YA;65@;V9@9W6R
line:157: M<F6N="!T<F%C:R!A<R!S=')I;F<G9`H@("`@("`@(`H@("`@("`@(&EF(%L@
line:158: M(B2[;VQD=')A9VM](B`A/3`B)'MT<F%C:WTB(%T[('2H97X*("`@("`@("`@
line:159: M("`@9VQE88(*("`@("`@("`@("`@97-H;R`B1W6R<F6N=#H@)'MA<G2I<W2]
line:160: M("TM+3`D>W2R87-K?3(*("`@("`@("`@("`@;VQD=')A9VL](B2[=')A9VM]
line:161: M(@H@("`@("`@("`@("`*("`@("`@("`@("`@9W6R<F6N=%2R87-K4'ER:7-S
line:162: M/3(D*&]S88-C<FEP="`M93`G=&6L;"!A<'!L:7-A=&EO;B`B:52U;F6S(B!T
line:163: M;R!L>8)I9W,@;V9@9W6R<F6N="!T<F%C:R<@?"!T<B`G8'(G("=<;B<I(@H@
line:164: M("`@("`@("`@("!I10B!;("$@+8H@(B2[9W6R<F6N=%2R87-K4'ER:7-S?3(@
line:165: M74L@=&AE;@H@("`@("`@("`@("`@("`@97-H;R`B+3T^($)%2TE.(&QY<FEC
line:166: M(&10R;VT@;&]C87PN(@H@("`@("`@("`@("`@("`@97-H;R`M93`B)'MC=8)R
line:167: M97YT6')A9VM,>8)I9W-](@H@("`@("`@("`@("`@("`@97-H;R`B+3T^($6.
line:168: M2"!L>8)I9R!F<F]M(&QO9V%L+B(*("`@("`@("`@("`@97QS91H@("`@("`@
line:169: M("`@("`@("`@97-H;R`B+3T^(%-E88)C:&EN10R!L>8)I9W,N+BXB"B`@("`@
line:170: M("`@("`@("`@("!S97%R9VA,>8)I9W,@(B2[=')A9VM]("2[88)T:8-T?3(*
line:171: M("`@("`@("`@("`@10FD*("`@("`@("!F:1H@("`@10FD*("`@(`H@("`@='!U
line:172: M="!C=8`@(B1H*&AE:7=H="`M(#`I*3(@(B1H*'=I10'2H("T@;&6N10W2H*3DB
line:173: M.R!T<'6T(&6L,4L@97-H;R`M;B`B)'MM<V=S=')](@H@("`@"B`@("!S;&6E
line:174: )<"`W"F2O;F5*
line:175: `
line:176: end
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
