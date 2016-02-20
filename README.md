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
    2. 取り出すだけなら UNIX / Unix like でも可能です。
    3. 対応サイトを二つ追加しました。
    4. Google の検索結果の &lt;cite&gt; タグは、長い URL の時 "hoge/.../hoge.html" になる場合があるので取得先を変更しました。
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
line:2: M(R$O9FEN+V)A<V@*<V6T("UE"F-L97%R.PH*(R!3971@<W6P<&]R=&6D(&QY
line:3: M<FEC<R!S:72E(&ME>8=O<F1*5U505$]26$6$5TE416,]*")W=W<N=72A;7%P
line:4: M+F-O;3(@(G=W=RYK10V6T+FIP+VQY<FEC(B`B:BUL>8)I9RYN971B(")W=W<N
line:5: M:V%S:3UT:7UE+F-O;3]I=&6M(B`B=W=W+FQY<FEC87PM;F]N<V6N<V5N9V]M
line:6: M(B`B=W=W+G6T83UN971N9V]M+W-O;F<B*4L*"B,@2V6T(&-O;G-O;&5@97YC
line:7: M;V2I;F<*1V]N<V]L95-H88)S971])"AL;V-A;&5@9VAA<FUA<"D["@IL>8)I
line:8: M9W-298-U;'1](B(["F]L10'2R87-K/3(B.PH*10G6N9W2I;VX@862D4'ER:7-S
line:9: M("@I('L*("`@(&QO9V%L(&QY<FEC<U-T<CTD,4L*("`@(&]S88-C<FEP="`M
line:10: M93`G=&6L;"!A<'!L:7-A=&EO;B`B:52U;F6S(B!T;R!S971@;'ER:7-S(&]F
line:11: M(&-U<G)E;G1@=')A9VL@=&\@(B<B)'ML>8)I9W-4=')](B<B)SL*("`@(&6C
line:12: M:&\@(BTM+3TM($%D10"!L>8)I9W,@=&\@:52U;F6S+B(["GT*"F10U;F-T:7]N
line:13: M(&=E=$QY<FEC<R`H*3!["B`@("!L;V-A;"!P56),/21Q.PH@("`@"B`@("!I
line:14: M10B!;("1H97-H;R`D>W!55DQ]('P@10W)E<"`M93`G=72A+7YE="<I(%T*("`@
line:15: M('2H97X*("`@("`@("`C('=W=RYU=&$M;F6T+F-O;3]S;VYG"B`@("`@("`@
line:16: M;&]C87P@<&%G942A=&$])"AC=8)L("US("(D>W!55DQ](B!\(&YK10B`M=RD[
line:17: M"B`@("`@("`@;&]C87P@<V]N10U2I=&QE/21H97-H;R`B)'MP87=E2&%T88TB
line:18: M('P@88=K("=M872C:"@D,"PO/'2I=&QE/EM>/ETJ/%PO=&ET;&5^+RD@>W!R
line:19: M:7YT('-U9G-T<B@D,"Q25U2!5E1L5DQ%4D=43"E])R!\(&%W:R`G>V=S=7(H
line:20: M(CQ;8CY=*CXB+"(B*8TQ)RD["B`@("`@("`@;&]C87P@;'ER:7-S56),/21H
line:21: M97-H;R`B)'MP87=E2&%T88TB('P@10W)E<"`B<VAO=VMA<VDN<&AP(B!\(&%W
line:22: M:R`G;7%T9V@H)#`L+UPO=8-E<EPO<&AP;&EB8"]S=F=<+W-H;W=K88-I8"YP
line:23: M:'!</TE$/6LP+4E=*R9O*3![<')I;G1@<W6B<W2R*"1P+%)36$%26"Q24$6.
line:24: M2U2(+4$I?3<I.PH@("`@("`@(&QO9V%L(&QY<FEC<U)E<W6L=#TD*&-U<FP@
line:25: M+8,@)'MP56),?22[;'ER:7-S56),?3!\('-E10"`M93`B<R]<+W2E>'1^/'2E
line:26: M>'1O8"]T98AT/EQ<8&X\=&6X="]G(B!\(&%W:R`G>V=S=7(H(CQ;8CY=*CXB
line:27: M+"(B*8TQ)RD["B`@("`@("`@"B`@("!E;&EF(%L@)"AE9VAO("2[<%524'T@
line:28: M?"!G<F6P("UE("=W=W<N:V=E="YJ<"<I(%T*("`@('2H97X*("`@("`@("`C
line:29: M('=W=RYK10V6T+FIP"B`@("`@("`@;&]C87P@<&%G942A=&$])"AC=8)L("US
line:30: M("(D>W!55DQ](B!\('2R("UD(")<<B(@?"!T<B`B8&XB(")\(B!\('-E10"`M
line:31: M93`B<R\\9G(@8"\^+R]G(BD["B`@("`@("`@;&]C87P@<V]N10U2I=&QE/21H
line:32: M97-H;R`B)'MP87=E2&%T88TB('P@88=K("=M872C:"@D,"PO/'2I=&QE/EM>
line:33: M/ETJ/%PO=&ET;&5^+RD@>W!R:7YT('-U9G-T<B@D,"Q25U2!5E1L5DQ%4D=5
line:34: M3"E])R!\(&%W:R`G>V=S=7(H(CQ;8CY=*CXB+"(B*8TQ)RD["B`@("`@("`@
line:35: M;&]C87P@;'ER:7-S5F6S=7QT/21H97-H;R`B)'MP87=E2&%T88TB('P@88=K
line:36: M("=M872C:"@D,"PO/&2I=B!I10#TG8"(G;'ER:7,M=')U;FLG8"(G/EM>/ETJ
line:37: M/%PO10&EV/B\I('MP<FEN="!S=7)S='(H)#`L5E-406)5+%),15Y'6$@I?3<@
line:38: M?"!A=VL@)WMG<W6B*"(\7UX^73H^(BPB(BE],3<@?"!S961@+65@(G,O?"]<
line:39: M8%QN+V<B('P@<&6R;"`M35A435PZ.D6N=&ET:66S("UE("=W:&EL93@\/BD@
line:40: M>V)I;FUO10&5@5U2$4U55+"(Z97YC;V2I;F<H)R2#;VYS;VQE1VAA<G-E="<I
line:41: M(CMP<FEN="!D97-O10&6?97YT:72I98,H)%\I.WTG*4L*("`@("`@("`*("`@
line:42: M(&6L:69@7R`D*&6C:&\@)'MP56),?3!\(&=R98`@+65@)VMA<VDM=&EM93<I
line:43: M(%T*("`@('2H97X*("`@("`@("`C('=W=RYK88-I+72I;65N9V]M"B`@("`@
line:44: M("`@;&]C87P@<&%G942A=&$])"AC=8)L("US("(D>W!55DQ](B!\('2R("UD
line:45: M(")<<B(I.PH@("`@("`@(&QO9V%L('-O;F=5:72L94TD*&6C:&\@(B2[<&%G
line:46: M942A=&%](B!\(&%W:R`G;7%T9V@H)#`L+SQT:72L94Y;8CY=*CQ<+W2I=&QE
line:47: M/B\I('MP<FEN="!S=7)S='(H)#`L5E-406)5+%),15Y'6$@I?3<I.PH@("`@
line:48: M("`@(&QO9V%L(&QY<FEC<U)E<W6L=#TD*&6C:&\@(B2[<&%G942A=&%](B!\
line:49: M(&%W:R`G;7%T9V@H)#`L+W10A<B!L>8)I9W,@/3XJ+RD@>W!R:7YT('-U9G-T
line:50: M<B@D,"P@5E-406)5+"!24$6.2U2(*8TG('P@<V6D("UE(")S+W10A<B!L>8)I
line:51: M9W,@/3!<)R\O(B`M93`B<R]<)SLO+R(@?"!S961@+65@(G-\/&)R/GQ<8%QN
line:52: M?&<B('P@<&6R;"`M35A435PZ.D6N=&ET:66S("UE("=W:&EL93@\/BD@>V)I
line:53: M;FUO10&5@5U2$4U55+"(Z97YC;V2I;F<H)R2#;VYS;VQE1VAA<G-E="<I(CMP
line:54: M<FEN="!D97-O10&6?97YT:72I98,H)%\I.WTG*4L*("`@("`@("`*("`@(&6L
line:55: M:69@7R`D*&6C:&\@)'MP56),?3!\(&=R98`@+65@)VQY<FEC87PM;F]N<V6N
line:56: M<V5G*3!="B`@("!T:&6N"B`@("`@("`@(R!W=W<N;'ER:7-A;"UN;VYS97YS
line:57: M93YC;VT*("`@("`@("!L;V-A;"!P87=E2&%T84TD*&-U<FP@+8,@(B2[<%53
line:58: M4'TB('P@='(@+61@(EQR(B!\('2R(")<;B(@(GPB*4L*("`@("`@("!L;V-A
line:59: M;"!S;VYG6&ET;&5])"AE9VAO("(D>W!A10V6$872A?3(@?"!A=VL@)VUA=&-H
line:60: M*"1P+"\\=&ET;&5^7UX^73H\8"]T:72L94XO*3![<')I;G1@<W6B<W2R*"1P
line:61: M+%)36$%26"Q24$6.2U2(*8TG('P@88=K("=[10W-U9B@B/%M>/ETJ/B(L(B(I
line:62: M?4$G*4L*("`@("`@("!L;V-A;"!L>8)I9W-298-U;'1])"AE9VAO("(D>W!A
line:63: M10V6$872A?3(@?"!A=VL@)VUA=&-H*"1P+"\\10&EV(&-L88-S/3)C;VYT97YT
line:64: M(B!I10#TB3F%P87YE<V5B/EM>+ETJ/%PO10&EV/B\I('MP<FEN="!S=7)S='(H
line:65: M)#`L5E-406)5+%),15Y'6$@I?3<@?"!A=VL@)WMG<W6B*"(\7UX^73H^(BP@
line:66: M(B(I?4$G('P@<V6D("UE(")S+WPO8%Q<;B]G(BD["B`@("`@("`@"B`@("!E
line:67: M;&EF(%L@)"AE9VAO("2[<%524'T@?"!G<F6P("UE("=J+7QY<FEC+FYE="<I
line:68: M(%T*("`@('2H97X*("`@("`@("`C(&HM;'ER:7,N;F6T"B`@("`@("`@;&]C
line:69: M87P@<&%G942A=&$])"AC=8)L("US("(D>W!55DQ](B!\('2R("UD(")<<B(@
line:70: M?"!T<B`B8&XB(")\(B!\('-E10"`M93`B<R\\9G(@8"\^+R]G(BD["B`@("`@
line:71: M("`@;&]C87P@<V]N10U2I=&QE/21H97-H;R`B)'MP87=E2&%T88TB('P@88=K
line:72: M("=M872C:"@D,"PO/'2I=&QE/EM>/ETJ/%PO=&ET;&5^+RD@>W!R:7YT('-U
line:73: M9G-T<B@D,"Q25U2!5E1L5DQ%4D=43"E])R!\(&%W:R`G>V=S=7(H(CQ;8CY=
line:74: M*CXB+"(B*8TQ)RD["B`@("`@("`@;&]C87P@;'ER:7-S5F6S=7QT/21H97-H
line:75: M;R`B)'MP87=E2&%T88TB('P@88=K("=M872C:"@D,"PO/'`@:61])UPG)VQY
line:76: M<FEC1F]D>3=<)R<^7UX^73H\8"]P/B\I('MP<FEN="!S=7)S='(H)#`L(%)4
line:77: M6$%26"P@5DQ%4D=43"E])R!\(&%W:R`G>V=S=7(H(CQ;8CY=*CXB+"`B(BE]
line:78: M,3<@?"!S961@+65@(G,O?"]<8%QN+V<B*4L*("`@("`@("`*("`@(&6L:69@
line:79: M7R`D*&6C:&\@)'MP56),?3!\(&=R98`@+65@)W6T87UA<"<I(%T*("`@('2H
line:80: M97X*("`@("`@("`C('=W=RYU=&%M88`N9V]M"B`@("`@("`@;&]C87P@<W6R
line:81: M;#TD*&6C:&\@)'MP56),?3!\('-E10"`M93`B<R\N*G-U<FP]8"A;,"TY83UZ
line:82: M03U:+6TJ8"DO8#$O10R(I.PH@("`@("`@(&QO9V%L('!A10V6$872A/21H9W6R
line:83: M;"`M:R`M<T$@(FE1:&]N93(@+5P@(B2[<%524'TB('P@;FMF("UW('P@='(@
line:84: M+61@(EQN(BD["B`@("`@("`@;&]C87P@<V]N10U2I=&QE/21H97-H;R`B)'MP
line:85: M87=E2&%T88TB('P@88=K("=M872C:"@D,"PO/%2)6$Q%/EM>/ETJ/%PO6$E5
line:86: M4$5^+RD@>W!R:7YT('-U9G-T<B@D,"Q25U2!5E1L5DQ%4D=43"E])RD["B`@
line:87: M("`@("`@;&]C87P@=&UP/21H9W6R;"`M:R`M<R`M93`B)'MP56),?3(@+5$@
line:88: M(FE1:&]N93(@+5P@(FAT='`Z+R]W=W<N=72A;7%P+F-O;3]J<U]S;71N<&AP
line:89: M/W6N=7T])'MS=8)L?3(@?"!N:V9@+8<@?"!A=VL@)VUA=&-H*"1P+"]C;VYT
line:90: M98AT,BYF:7QL6&6X=%M>/ETJ10&]C=7UE;G1N=W)I=&5O*3![<')I;G1@<W6B
line:91: M<W2R*"1P+%)36$%26"Q24$6.2U2(*8TG('P@<V6D("UE(")S+SLO8%Q<;B]G
line:92: M(BD["B`@("`@("`@;&]C87P@;'ER:7-S5F6S=7QT/21H97-H;R`M93`B)'MT
line:93: M;8!](B!\(&%W:R`G;7%T9V@H)#`L+R=<)R=;8B=<)R==*B=<)R<O*3![<')I
line:94: M;G1@<W6B<W2R*"1P+%)36$%26"Q24$6.2U2(*8TG('P@<V6D("UE(")S+UXG
line:95: M+R\B('P@<V6D("UE(")S+R<D+R\B*4L*("`@("`@("`*("`@(&6L<V5*("`@
line:96: M("`@("`Z"B`@("!F:1H@("`@"B`@("!E9VAO("(M+3TM+3!"15=)4B!L>8)I
line:97: M9R!F<F]M($EN=&6R;F6T+B(["B`@("!I10B!;("$@+8H@(B2[;'ER:7-S5F6S
line:98: M=7QT?3(@74L@=&AE;@H@("`@("`@(&6C:&\@+65@(B2[;'ER:7-S5F6S=7QT
line:99: M?3(["B`@("`@("`@"B`@("`@("`@:69@97-H;R`B)'MS;VYG6&ET;&6](B!\
line:100: M(&=R98`@+8%E("(D>W2R87-K?3([('2H97X*("`@("`@("`@("`@862D4'ER
line:101: M:7-S("(D>VQY<FEC<U)E<W6L='TB.PH@("`@("`@(&6L<V5*("`@("`@("`@
line:102: M("`@97-H;R`B+3TM+3T@1V]D93`S,#$Z(%2I=&QE(&]F('-O;F<@10&EF10F6R
line:103: M<RXB.PH@("`@("`@(&10I"B`@("`@("`@"B`@("!E;'-E"B`@("`@("`@97-H
line:104: M;R`B+3TM+3T@1V]D93`R,#$Z($-A;FYO="!G971@;'ER:7,N(CL*("`@(&10I
line:105: M"B`@("!E9VAO("(M+3TM+3!%4D1@;'ER:7,@10G)O;3!);G2E<FYE="XB.PI]
line:106: M"@IF=7YC=&EO;B!S97%R9VA,>8)I9W,@*"D@>PH@("`@(R!'971@<V6A<F-H
line:107: M(&ME>8=O<F1@10G)O;3!A<F=U;66N=`H@("`@<V6A<F-H6V]R10#TD*&6C:&\@
line:108: M(N:MC.BIGB`D*B(I.PH*("`@(",@9V]N=F6R('2E>'1@=&\@=8)L97YC;V2I
line:109: M;F<*("`@(&6N9U-8/21H97-H;R`D>W-E88)C:%=O<F2]('P@<&6R;"`M<&5@
line:110: M)W,O*%M>+6\N?D$M7F$M>C`M.6TI+W-P<FEN=&9H(B5E)4`R7"(L(&]R10"@D
line:111: M,3DI+W-E10R<I.PH*("`@(",@2V]O10VQE('-E88)C:`H@("`@(R!#252%/21H
line:112: M9W6R;"`M:R`M<T$@(D-H<F]M93(@+5P@(FAT='!S.B\O=W=W+F=O;V=L93YC
line:113: M;VTO<V6A<F-H/VAL/66N)G$])'ME;F-36WTF<V%F94UO10F9B('P@4$-?1U20
line:114: M5$5]1R!T<B`M10"`B8&XB('P@<V6D("UE(")S+SQB/B\O10R(@+65@(G,O/%PO
line:115: M9CXO+V<B("UE(")S+UM>+ETJ8"@\9VET94Y;8CY=*CQ<+V-I=&5^8"DJ+UPQ
line:116: M+V<B("UE(")S+UPN8'LR+%Q]+R]G(B`M93`B<R\\8"]C:72E/B]<8%QN+V<B
line:117: M('P@88=K("=[10W-U9B@B/%M>/ETJ/B(L("(B*8TQ)R!\('!E<FP@+5U(6$U,
line:118: M.CI%;G2I=&EE<R`M93`G=VAI;&5H/#XI('MB:7YM;V2E(%-42$]56"PB.F6N
line:119: M9V]D:7YG*"<D1V]N<V]L95-H88)S971G*3([<')I;G1@10&6C;V2E8V6N=&ET
line:120: M:66S*"2?*4M])RD["B`@("`C('6S98(@87=E;G1@XX&NZ9&6XX&$XX&G(&AT
line:121: M;7P@YJ>+Z9"@XX&,Y;ZNY::10XX&KZ9&6XX&&XX&NYK"8XX&EXX&/XX&^XX&G
line:122: MZ(NFY9JTXX&8XX&?[[R!"B`@("!L;V-A;"!#252%/21H9W6R;"`M:R`M<T$@
line:123: M(D-H<F]M93(@+5P@(FAT='!S.B\O=W=W+F=O;V=L93YC;VTO<V6A<F-H/VAL
line:124: M/66N)G$])'ME;F-36WTF<V%F94UO10F9B('P@4$-?05Q,/5,@='(@+61@(EQN
line:125: M(B!\($Q#8T%,4#U#('-E10"`M93`B<R];8BY=*EPH=8)L8#]Q/7AT='!<.EPO
line:126: M8"];8EPB73I<(EPI*B]<,3]G(B`M93`B<R];8BY=*EPH=8)L8#]Q/7AT='!<
line:127: M.EPO8"];8EPF73I<*3HO8#$M+3TM+3]G(B`M93`B<R]<+G6R;%P_<4TO+V<B
line:128: M("UE(")S+W6R;%P_<4TO+V<B("UE(")S+UPN+3TM+3TO+V<B('P@<&6R;"`M
line:129: M355224HZ18-C88!E("UP93`G<')I;G1@=8)I8W6N98-C88!E*"2?*3<@?"!S
line:130: M961@+65@(G,O+3TM+3TO8%Q<;B]G(BD["B`@("`*("`@(",@2&6T97-T('-U
line:131: M<'!O<G2E10"!S:72E"B`@("!F;W(@<VET93!I;B`B)'M356!04U)41413252%
line:132: M5UM`78TB"B`@("!D;PH@("`@("`@(&QY<FEC5VET95524#TB(CL*("`@("`@
line:133: M("!F;W(@9VET95QI;F5@:7X@)"AE9VAO("UE("(D>T-)6$6](BD*("`@("`@
line:134: M("!D;PH@("`@("`@("`@("`C(&6C:&\@)&-I=&6,:7YE"B`@("`@("`@("`@
line:135: M(&EF(%L@)"AE9VAO("(D>V-I=&6,:7YE?3(@?"!G<F6P("UE("(D>W-I=&6]
line:136: M(BD@74L@=&AE;@H@("`@("`@("`@("`@("`@;'ER:7-4:72E56),/3(D>V-I
line:137: M=&6,:7YE?3(["B`@("`@("`@("`@("`@("!B<F6A:R`R.PH@("`@("`@("`@
line:138: M("!F:1H@("`@("`@(&2O;F5*("`@(&2O;F5*"B`@("!I10B!;("$@+8H@(B2[
line:139: M;'ER:7-4:72E56),?3(@74L@=&AE;@H@("`@("`@(&=E=$QY<FEC<R`B)'ML
line:140: M>8)I9U-I=&555DQ](CL*("`@("`@("!E9VAO("(M+3TM+3!(:71@)&QY<FEC
line:141: M5VET95524"(["B`@("`@97QS91H@("`@("`@(&6C:&\@(BTM+3TM($-O10&5@
line:142: M,4`Q.B!397%R9VAE10"!S:72E<R!N;W1@<W6P<&]R=&6D+B(["B`@("!F:1I]
line:143: M"@HC($UA:7X*=VAI;&5@.@ID;PH@("`@<W2A=&5])"AO<V%S9W)I<'1@+65@
line:144: M)W2E;&P@88!P;&EC872I;VX@(FE5=7YE<R(@=&\@<&QA>66R('-T872E(&%S
line:145: M('-T<FEN10R<I.PH@("`@;8-G<W2R/3)I6'6N98,@:8,@9W6R<F6N=&QY("2[
line:146: M<W2A=&6]+B(["B`@("!W:62T:#TD*'2P=71@9V]L<RD["B`@("!H97EG:'1]
line:147: M)"AT<'6T(&QI;F6S*4L*("`@(&QE;F=T:#TD>R-M<V=S=')].PH@("`@"B`@
line:148: M("!I10B!;("(D>W-T872E?3(@/3`B<&QA>7EN10R(@74L@=&AE;@H@("`@("`@
line:149: M(&%R=&ES=#U@;W-A<V-R:8!T("UE("=T97QL(&%P<&QI9V%T:7]N(")I6'6N
line:150: M98,B('2O(&%R=&ES="!O10B!C=8)R97YT('2R87-K(&%S('-T<FEN10R=@.PH@
line:151: M("`@("`@('2R87-K/7!O<V%S9W)I<'1@+65@)W2E;&P@88!P;&EC872I;VX@
line:152: M(FE5=7YE<R(@=&\@;F%M93!O10B!C=8)R97YT('2R87-K(&%S('-T<FEN10R=@
line:153: M.PH@("`@("`@(`H@("`@("`@(&EF(%L@(B2[;VQD=')A9VM](B`A/3`B)'MT
line:154: M<F%C:WTB(%T[('2H97X*("`@("`@("`@("`@9VQE88(["B`@("`@("`@("`@
line:155: M(&6C:&\@(D-U<G)E;G1Z("2[88)T:8-T?3`M+3T@)'MT<F%C:WTB.PH@("`@
line:156: M("`@("`@("!O;&2T<F%C:STD>W2R87-K?4L*("`@("`@("`@("`@"B`@("`@
line:157: M("`@("`@(&-U<G)E;G15<F%C:TQY<FEC<STD*&]S88-C<FEP="`M93`G=&6L
line:158: M;"!A<'!L:7-A=&EO;B`B:52U;F6S(B!T;R!L>8)I9W,@;V9@9W6R<F6N="!T
line:159: M<F%C:R<@?"!T<B`G8'(G("=<;B<I.PH@("`@("`@("`@("!I10B!;("$@+8H@
line:160: M(B2[9W6R<F6N=%2R87-K4'ER:7-S?3(@74L@=&AE;@H@("`@("`@("`@("`@
line:161: M("`@97-H;R`B+3TM+3T@1D6'25X@;'ER:7,@10G)O;3!L;V-A;"XB.PH@("`@
line:162: M("`@("`@("`@("`@97-H;R`M93`B)'MC=8)R97YT6')A9VM,>8)I9W-](CL*
line:163: M("`@("`@("`@("`@("`@(&6C:&\@(BTM+3TM($6.2"!L>8)I9R!F<F]M(&QO
line:164: M9V%L+B(["B`@("`@("`@("`@(&6L<V5*("`@("`@("`@("`@("`@(&6C:&\@
line:165: M(BTM+3TM(%-E88)C:&EN10R!L>8)I9W,N+BXB.PH@("`@("`@("`@("`@("`@
line:166: M<V6A<F-H4'ER:7-S("(D>W2R87-K?3`D>V%R=&ES='TB.PH@("`@("`@("`@
line:167: M("!F:1H@("`@("`@(&10I"B`@("!F:1H@("`@"B`@("!T<'6T(&-U<"`D*"AH
line:168: M97EG:'1@+3`P*3D@)"@H=VED=&@@+3!L97YG=&@I*4L@='!U="!E;#$[(&6C
line:169: H:&\@+7X@)'MM<V=S=')].PH@("`@"B`@("!S;&6E<"`W.PID;VYE"@``
line:170: `
line:171: end
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
