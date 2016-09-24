# バシュっと歌詞のテキストデータを取得できる bash スクリプト

## お知らせ

問 3 と 4 を追加しました。問 3 は ver3 になり新しい機能や対応サイトを追加しました。詳細は下記仕様をご覧下さい。なお、問 3 は簡単なひっかけ問題になっています。


## 概要

これは、いくつかの歌詞表示サイトからテキストな歌詞データを取得する bash スクリプトです。スクリプトは PNG 画像に埋め込んだり、文字列にエンコードしたりしています。これらはワンライナで取り出すことができます。暇だったら挑戦してみてください。取り出したスクリプトにはド定番のコードからこの用途でしか使わない正規表現が入っているので、シェルおじさんになりたいマンやシェルに興味ありウーマンの方々が挑戦してくれると嬉しいです。

なお、現状洋楽には弱いです。壊滅的です。今後対応します。対応サイトも増やす予定です。

!["ver 2 スクショ"](./ScreenShot.png)

埋め込んであるスクリプトは GNU 系を回避したはずなので、UNIX / Unix like で動くと思います。当方 Sierra と Raspbian で動作確認しています。


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
11. なお...Google safe browsing がどうしても on になってしまい、例えば「ポリスマンファック」は検索結果の時点で「ポリスマンベンツ」になってしまいます。(曲名が異なるので iTunes には保存されません) (&amp;safe=off にしてるんだけどなあ... cookie 食わせないとダメかあ...)
12. 一部のサイトが CP932, X0213 ですら UTF-8 に変換できない文字を使っているので、iconv から nkf に変更しました。(brew install nkf してからご使用ください)

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
line:2: M(R$O9FEN+V)A<V@*(PHC(%2H93!-251@4&EC97YS93`H35E5*1HC($-O<'ER
line:3: M:7=H="`H9RD@,C`Q-B!T:W6M872A"B,*(R!098)M:8-S:7]N(&ES(&AE<F6B
line:4: M>3!G<F%N=&6D+"!F<F6E(&]F(&-H88)G93P@=&\@87YY('!E<G-O;B!O9G2A
line:5: M:7YI;F<@83!C;W!Y"B,@;V9@=&AI<R!S;V10T=V%R93!A;F1@88-S;V-I872E
line:6: M10"!D;V-U;66N=&%T:7]N(&10I;&6S("AT:&5@(E-O10G2W88)E(BDL('2O(&2E
line:7: M87P*(R!I;B!T:&5@5V]F='=A<F5@=VET:&]U="!R98-T<FEC=&EO;BP@:7YC
line:8: M;'6D:7YG('=I=&AO=71@;&EM:72A=&EO;B!T:&5@<FEG:'2S"B,@=&\@=8-E
line:9: M+"!C;W!Y+"!M;V2I10GDL(&UE<F=E+"!P=7)L:8-H+"!D:8-T<FEB=72E+"!S
line:10: M=7)L:7-E;G-E+"!A;F1O;W(@<V6L;`HC(&-O<&EE<R!O10B!T:&5@5V]F='=A
line:11: M<F5L(&%N10"!T;R!P98)M:71@<&6R<V]N<R!T;R!W:&]M('2H93!4;V10T=V%R
line:12: M93!I<PHC(&10U<FYI<VAE10"!T;R!D;R!S;RP@<W6B:F6C="!T;R!T:&5@10F]L
line:13: M;&]W:7YG(&-O;F2I=&EO;G,Z"B,*(R!5:&5@87)O=F5@9V]P>8)I10VAT(&YO
line:14: M=&EC93!A;F1@=&AI<R!P98)M:8-S:7]N(&YO=&EC93!S:&%L;"!B93!I;F-L
line:15: M=62E10"!I;@HC(&%L;"!C;W!I98,@;W(@<W6B<W2A;G2I87P@<&]R=&EO;G,@
line:16: M;V9@=&AE(%-O10G2W88)E+@HC"B,@6$A%(%-/2E1706)%($E4(%!24U10)2$6$
line:17: M(")!5R!)5R(L(%=)6$A/551@6T%25D%.6%D@4T9@05Y10($M)4D1L($585%)%
line:18: M5U,@4U(*(R!)36!,246$+"!)4D-,542)4D<@1E55($Y/6"!,25U)6$6$(%2/
line:19: M(%2(13!706)205Y42454($]&($U%5D-(05Y405))4$E463P*(R!&252.16-4
line:20: M($10/5B!!(%!!5E2)1U6,06(@5%525$]313!!4D1@4D].25Y&5DE.2T6-15Y5
line:21: M+B!)4B!.4R!%6D6.6"!33$%,4"!43$5*(R!!552(4U)4($]3($-/5%E225=(
line:22: M6"!(4TQ$16)4($)%($Q)05),13!&4U(@05Y10($-,05E-+"!$05U!2T54($]3
line:23: M($]43$53"B,@4$E!1DE,2520+"!73$543$53($E.($%.($%#6$E/4B!/2B!#
line:24: M4TY45D%#6"P@6$]26"!/5B!/6$A%5E=)5T5L($%226-)4D<@2E)/33P*(R!/
line:25: M551@4T9@4U(@25X@1T].4D6#6$E/4B!7252((%2(13!34T946T%213!/5B!5
line:26: M3$5@56-%($]3($]43$53($2%05Q)4D=4($E.(%2(11HC(%-/2E1706)%+@HC
line:27: M"B,@<V6T("UE"F-L97%R"@IK98)N8VYA;65](B1H=7YA;65@+7$@?"!A=VL@
line:28: M)WMP<FEN="`D,8TG*3(*"FEF(%L@(B2K98)N8VYA;65B("$](")$88)W:7XB
line:29: M(%T[('2H97X*("`@(&6C:&\@(E!L97%S93!R=7X@;VX@4U,@7"XB"B`@("!E
line:30: M>&ET(#$*10FD*"B,@5V6T('-I=&5@=VAI9V@@<W6P<&]R="!L>8)I9W,*5U51
line:31: M5$]26$6$5TE416,]*")W=W<N=72A;7%P+F-O;3(@(G=W=RYK88-I+72I;65N
line:32: M9V]M+VET97TB(")J+7QY<FEC+FYE="(@(G=W=RYK10V6T+FIP+VQY<FEC(B`B
line:33: M=W=W+G6T83UN971N9V]M+W-O;F<B*1HC5U505$]26$6$5TE416,]*")W=W<N
line:34: M=72A;7%P+F-O;3(@(G=W=RYK88-I+72I;65N9V]M+VET97TB(")W=W<N:V=E
line:35: M="YJ<"]L>8)I9R(@(FHM;'ER:7,N;F6T(B`B=W=W+G6T83UN971N9V]M+W-O
line:36: M;F<B(")W=W<N;'ER:7-A;"UN;VYS97YS93YC;VTB*1H*(R!'971@9V]N<V]L
line:37: M93!E;F-O10&EN10PI#;VYS;VQE1VAA<G-E=#TB)"AL;V-A;&5@9VAA<FUA<"DB
line:38: M"@HC($EN:71N"DQ95DE#8U)%5U6,6#TB(@I/4$2?6%)!1TL](B(*"B,@062D
line:39: M:7YG(&QY<FEC('2O(&E5=7YE<RX*10G6N9W2I;VX@862D4'ER:7-S*"D@>PH@
line:40: M("`@;&]C87P@;'ER:7-S5W2R/3(D,3(*("`@(&]S88-C<FEP="`M93`G=&6L
line:41: M;"!A<'!L:7-A=&EO;B`B:52U;F6S(B!T;R!S971@;'ER:7-S(&]F(&-U<G)E
line:42: M;G1@6')A9VL@=&\@(B<B)'ML>8)I9W-4=')](B<B)PH@("`@97-H;R`B+3TM
line:43: M+3T@062D(&QY<FEC<R!T;R!I6'6N98,N(@I]"@HC($=E='2I;F<@;'ER:7,N
line:44: M"F10U;F-T:7]N(&=E=$QY<FEC<R@I('L*("`@(&QO9V%L('!55DP])#$*("`@
line:45: M(`H@("`@:69@7R`D*&6C:&\@(B2[<%524'TB('P@10W)E<"`M93`G=72A+7YE
line:46: M="<I(%T[('2H97X*("`@("`@("`C('=W=RYU=&$M;F6T+F-O;3]S;VYG"B`@
line:47: M("`@("`@;&]C87P@<&%G96]D872A/21H9W6R;"`M<R`B)'MP56),?3(@?"!N
line:48: M:V9@+8<I"B`@("`@("`@;&]C87P@<V]N10U]T:72L94TD*&6C:&\@(B2[<&%G
line:49: M96]D872A?3(@?"!<"B`@("`@("`@("`@(&%W:R`G;7%T9V@H)#`L("\\=&ET
line:50: M;&5^7UX^73H\8"]T:72L94XO*3![<')I;G1@<W6B<W2R*"1P+"!25U2!5E1L
line:51: M(%),15Y'6$@I?3<@?"!<"B`@("`@("`@("`@(&%W:R`G>V=S=7(H(CQ;8CY=
line:52: M*CXB+"`B(BE],3<I"B`@("`@("`@;&]C87P@;'ER:7-?56),/21H97-H;R`B
line:53: M)'MP87=E8V2A=&%](B!\(&=R98`@(G-H;W=K88-I+G!H<"(@?"!<"B`@("`@
line:54: M("`@("`@(&%W:R`G;7%T9V@H)#`L("]<+W6S98)<+W!H<&QI9EPO<W10G8"]S
line:55: M:&]W:V%S:6PN<&AP8#])2#U;,"TY73LF+RD@>W!R:7YT('-U9G-T<B@D,"P@
line:56: M5E-406)5+"!24$6.2U2(+4$I?3<I"B`@("`@("`@;&]C87P@4%E225-?5D54
line:57: M55Q5/21H9W6R;"`M<R`B)'MP56),?22[;'ER:7-?56),?3(@?"!<"B`@("`@
line:58: M("`@("`@('-E10"`M93`B<R]<+W2E>'1^/'2E>'1O8"]T98AT/EQ<8&X\=&6X
line:59: M="]G(B!\(%P*("`@("`@("`@("`@88=K("=[10W-U9B@B/%M>/ETJ/B(L("(B
line:60: M*8TQ)RD*"B`@("!E;&EF(%L@)"AE9VAO("(D>W!55DQ](B!\(&=R98`@+65@
line:61: M)W=W=RYK10V6T+FIP)RD@74L@=&AE;@H@("`@("`@(",@=W=W+FMG971N:G`*
line:62: M("`@("`@("!L;V-A;"!P87=E8V2A=&$])"AC=8)L("US("(D>W!55DQ](B!\
line:63: M('2R("UD(")<<B(@?"!T<B`B8&XB(")\(B!\('-E10"`M93`B<R\\9G(@8"\^
line:64: M+R]G(BD*("`@("`@("!L;V-A;"!S;VYG8W2I=&QE/21H97-H;R`B)'MP87=E
line:65: M8V2A=&%](B!\(%P*("`@("`@("`@("`@88=K("=M872C:"@D,"P@+SQT:72L
line:66: M94Y;8CY=*CQ<+W2I=&QE/B\I('MP<FEN="!S=7)S='(H)#`L(%)36$%26"P@
line:67: M5DQ%4D=43"E])R!\(%P*("`@("`@("`@("`@88=K("=[10W-U9B@B/%M>/ETJ
line:68: M/B(L("(B*8TQ)RD*("`@("`@("!L;V-A;"!,66))1U]216-54%1])"AE9VAO
line:69: M("(D>W!A10V6?10&%T88TB('P@8`H@("`@("`@("`@("!A=VL@)VUA=&-H*"1P
line:70: M+"`O/&2I=B!I10#TG8"(G;'ER:7,M=')U;FLG8"(G/EM>/ETJ/%PO10&EV/B\I
line:71: M('MP<FEN="!S=7)S='(H)#`L(%)36$%26"P@5DQ%4D=43"E])R!\(%P*("`@
line:72: M("`@("`@("`@88=K("=[10W-U9B@B/%M>/ETJ/B(L("(B*8TQ)R!\(%P*("`@
line:73: M("`@("`@("`@<V6D("UE(")S+WPO8%Q<;B]G(B!\(%P*("`@("`@("`@("`@
line:74: M<&6R;"`M35A435PZ.D6N=&ET:66S("UE("=W:&EL93@\/BD@>V)I;FUO10&5@
line:75: M5U2$4U55+"(Z97YC;V2I;F<H)R2#;VYS;VQE1VAA<G-E="<I(CL@<')I;G1@
line:76: M10&6C;V2E8V6N=&ET:66S*"2?*4M])RD*"B`@("!E;&EF(%L@)"AE9VAO("(D
line:77: M>W!55DQ](B!\(&=R98`@+65@)VMA<VDM=&EM93<I(%T[('2H97X*("`@("`@
line:78: M("`C('=W=RYK88-I+72I;65N9V]M"B`@("`@("`@;&]C87P@<&%G96]D872A
line:79: M/21H9W6R;"`M<R`B)'MP56),?3(@?"!T<B`M10"`B8'(B*1H@("`@("`@(&QO
line:80: M9V%L('2M<%]S;VYG8W2I=&QE/21H97-H;R`B)'MP87=E8V2A=&%](B!\(%P*
line:81: M("`@("`@("`@("`@88=K("=M872C:"@D,"P@+SQT:72L94Y;8CY=*CQ<+W2I
line:82: M=&QE/B\I('MP<FEN="!S=7)S='(H)#`L(%)36$%26"P@5DQ%4D=43"E])R!\
line:83: M(%P*("`@("`@("`@("`@88=K("=[10W-U9B@B/%M>/ETJ/B(L("(B*8TQ)RD*
line:84: M("`@("`@("!L;V-A;"!S;VYG8W2I=&QE/3(D>W2M<%]S;VYG8W2I=&QE)3`M
line:85: M("I](@H@("`@("`@(&QO9V%L($Q95DE#8U)%5U6,6#TD*&6C:&\@(B2[<&%G
line:86: M96]D872A?3(@?"!<"B`@("`@("`@("`@(&%W:R`G;7%T9V@H)#`L("]V88(@
line:87: M;'ER:7-S(#TN*B\I('MP<FEN="!S=7)S='(H)#`L(%)36$%26"P@5DQ%4D=5
line:88: M3"E])R!\(%P*("`@("`@("`@("`@<V6D("UE(")S+W10A<B!L>8)I9W,@/3!<
line:89: M)R\O(B!<"B`@("`@("`@("`@("`@("`M93`B<R]<)SLO+R(@8`H@("`@("`@
line:90: M("`@("`@("`@+65@(G-\/&)R/GQ<8%QN?&<B('P@8`H@("`@("`@("`@("!P
line:91: M98)L("U-3%2-4#HZ17YT:72I98,@+65@)W=H:7QE*#P^*3![9FEN;7]D93!4
line:92: M6$2/551L(CIE;F-O10&EN10R@G)$-O;G-O;&6#:&%R<V6T)RDB.R!P<FEN="!D
line:93: M97-O10&6?97YT:72I98,H)%\I.WTG*1H*("`@(&6L:69@7R`D*&6C:&\@(B2[
line:94: M<%524'TB('P@10W)E<"`M93`G;'ER:7-A;"UN;VYS97YS93<I(%T[('2H97X*
line:95: M("`@("`@("`C('=W=RYL>8)I9V%L+7YO;G-E;G-E+F-O;1H@("`@("`@(&QO
line:96: M9V%L('!A10V6?10&%T84TD*&-U<FP@+8,@(B2[<%524'TB('P@='(@+61@(EQR
line:97: M(B!\('2R(")<;B(@(GPB*1H@("`@("`@(&QO9V%L('-O;F=?=&ET;&5])"AE
line:98: M9VAO("(D>W!A10V6?10&%T88TB('P@8`H@("`@("`@("`@("!A=VL@)VUA=&-H
line:99: M*"1P+"`O/'2I=&QE/EM>/ETJ/%PO=&ET;&5^+RD@>W!R:7YT('-U9G-T<B@D
line:100: M,"P@5E-406)5+"!24$6.2U2(*8TG('P@8`H@("`@("`@("`@("!A=VL@)WMG
line:101: M<W6B*"(\7UX^73H^(BP@(B(I?4$G*1H@("`@("`@(&QO9V%L($Q95DE#8U)%
line:102: M5U6,6#TD*&6C:&\@(B2[<&%G96]D872A?3(@?"!<"B`@("`@("`@("`@(&%W
line:103: M:R`G;7%T9V@H)#`L("\\10&EV(&-L88-S/3)C;VYT97YT(B!I10#TB3F%P87YE
line:104: M<V5B/EM>+ETJ/%PO10&EV/B\I('MP<FEN="!S=7)S='(H)#`L(%)36$%26"P@
line:105: M5DQ%4D=43"E])R!\(%P*("`@("`@("`@("`@88=K("=[10W-U9B@B/%M>/ETJ
line:106: M/B(L("(B*8TQ)R!\(%P*("`@("`@("`@("`@<V6D("UE(")S+WPO8%Q<;B]G
line:107: M(BD*("`@("`@("`@("`@"B`@("!E;&EF(%L@)"AE9VAO("(D>W!55DQ](B!\
line:108: M(&=R98`@+65@)VHM;'ER:7,N;F6T)RD@74L@=&AE;@H@("`@("`@(",@:BUL
line:109: M>8)I9RYN971*("`@("`@("!L;V-A;"!P87=E8V2A=&$])"AC=8)L("US("(D
line:110: M>W!55DQ](B!\('2R("UD(")<<B(@?"!T<B`B8&XB(")\(B!\('-E10"`M93`B
line:111: M<R\\9G(@8"\^+R]G(BD*("`@("`@("!L;V-A;"!S;VYG8W2I=&QE/21H97-H
line:112: M;R`B)'MP87=E8V2A=&%](B!\(%P*("`@("`@("`@("`@88=K("=M872C:"@D
line:113: M,"P@+SQT:72L94Y;8CY=*CQ<+W2I=&QE/B\I('MP<FEN="!S=7)S='(H)#`L
line:114: M(%)36$%26"P@5DQ%4D=43"E])R!\(%P*("`@("`@("`@("`@88=K("=[10W-U
line:115: M9B@B/%M>/ETJ/B(L("(B*8TQ)RD*("`@("`@("!L;V-A;"!,66))1U]216-6
line:116: M4%1])"AE9VAO("(D>W!A10V6?10&%T88TB('P@8`H@("`@("`@("`@("!A=VL@
line:117: M)VUA=&-H*"1P+"`O/'`@:61])UPG)VQY<FEC1F]D>3=<)R<^7UX^73H\8"]P
line:118: M/B\I('MP<FEN="!S=7)S='(H)#`L(%)36$%26"P@5DQ%4D=43"E])R!\(%P*
line:119: M("`@("`@("`@("`@88=K("=[10W-U9B@B/%M>/ETJ/B(L("(B*8TQ)R!\(%P*
line:120: M("`@("`@("`@("`@<V6D("UE(")S+WPO8%Q<;B]G(BD*"B`@("!E;&EF(%L@
line:121: M)"AE9VAO("(D>W!55DQ](B!\(&=R98`@+65@)W6T87UA<"<I(%T[('2H97X*
line:122: M("`@("`@("`C('=W=RYU=&%M88`N9V]M"B`@("`@("`@;&]C87P@<W6R;#TD
line:123: M*&6C:&\@(B2[<%524'TB('P@<V6D("UE(")S+RXJ<W6R;#U<*%LP+4EA+8I!
line:124: M+6HM73I<*3]<,3]G(BD*("`@("`@("!L;V-A;"!P87=E8V2A=&$])"AC=8)L
line:125: M("UK("US03`B:6!H;VYE(B`M4"`B)'MP56),?3(@?"!N:V9@+8<@?"!T<B`M
line:126: M10"`B8&XB*1H@("`@("`@(&QO9V%L('2M<%]S;VYG8W2I=&QE/21H97-H;R`B
line:127: M)'MP87=E8V2A=&%](B!\(%P*("`@("`@("`@("`@88=K("=M872C:"@D,"P@
line:128: M+SQ4252,14Y;8CY=*CQ<+U2)6$Q%/B\I('MP<FEN="!S=7)S='(H)#`L(%)4
line:129: M6$%26"P@5DQ%4D=43"E])R!\(%P*("`@("`@("`@("`@88=K("=[10W-U9B@B
line:130: M/%M>/ETJ/B(L("(B*8TQ)RD*("`@("`@("!L;V-A;"!S;VYG8W2I=&QE/3(D
line:131: M>W2M<%]S;VYG8W2I=&QE)3\J?3(*("`@("`@("!L;V-A;"!T;8`])"AC=8)L
line:132: M("UK("US("UE("(D>W!55DQ](B`M03`B:6!H;VYE(B`M4"`B:'2T<#HO+W=W
line:133: M=RYU=&%M88`N9V]M+VIS8W-M="YP:'`_=7YU;4TD>W-U<FQ](B!\(&YK10B`M
line:134: M=R!\(%P*("`@("`@("`@("`@88=K("=M872C:"@D,"P@+V-O;G2E>'1R+F10I
line:135: M;&Q498AT7UX^73ID;V-U;66N="YW<FET93\I('MP<FEN="!S=7)S='(H)#`L
line:136: M(%)36$%26"P@5DQ%4D=43"E])R!\(%P*("`@("`@("`@("`@<V6D("UE(")S
line:137: M+SLO8%Q<;B]G(BD*("`@("`@("!L;V-A;"!,66))1U]216-54%1])"AE9VAO
line:138: M("UE("(D=&UP(B!\('-E10"`M93`B<R]<8%PG+U\O10R(@?"!<"B`@("`@("`@
line:139: M("`@(&%W:R`G;7%T9V@H)#`L("\G8"<G7UXG8"<G73HG8"<G+RD@>W!R:7YT
line:140: M('-U9G-T<B@D,"P@5E-406)5+"!24$6.2U2(*8TG('P@8`H@("`@("`@("`@
line:141: M("!S961@+65@(G,O8B<O+R(@+65@(G,O)R1O+R(I"B`@("!F:1H@("`@"B`@
line:142: M("!E9VAO("(M+3TM+3TM+3TM($)%2TE..B!,>8)I9W,@10G)O;3!);G2E<FYE
line:143: M="XB"B`@("`*("`@(&EF(%L@(3`M>B`B)'M,66))1U]216-54%2](B!=.R!T
line:144: M:&6N"B`@("`@("`@97-H;R`M93`B)'M,66))1U]216-54%2](@H@("`@("`@
line:145: M(`H@("`@("`@(",@6$]$4SH@<F6M;W10E('!L872F;W)M(&2E<&6N10&6N="!C
line:146: M:&%R87-T98)S(&10R;VT@)%1205-+8TY!345*("`@("`@("`C(&EF(&6C:&\@
line:147: M(B2[<V]N10U]T:72L98TB('P@10W)E<"`M:3`B)'M45D%#3U].05U%?3(@/B`O
line:148: M10&6V+VYU;&P@,CXF,4L@=&AE;@H@("`@("`@(&QO9V%L(&$]9&6C:&\@(B2[
line:149: M<V]N10U]T:72L98TB('P@<&6R;"`M;F5@(G!R:7YT(&EF("@O)'M45D%#3U].
line:150: M05U%?3\I(F`*("`@("`@("!I10B!;("$@+8H@(B2A(B!=.R!T:&6N"B`@("`@
line:151: M("`@("`@(&6C:&\@(BTM+3TM(%1205-+8TY!345@9V]N=&%I;G,@6V6B('2I
line:152: M=&QE+B(*("`@("`@("`@("`@862D4'ER:7-S("(D>TQ95DE#8U)%5U6,6'TB
line:153: M"B`@("`@("`@97QS91H@("`@("`@("`@("!E9VAO("(M+3TM+3!#;V2E(#1P
line:154: M,CH@6&ET;&5@10&EF10F6R<RXB"B`@("`@("`@("`@(&6C:&\@(E-I=&5@=&ET
line:155: M;&5Z("`@("2[<V]N10U]T:72L98TB"B`@("`@("`@("`@(&6C:&\@(D-U<G)E
line:156: M;G1@6%)!1TLZ("2[6%)!1TM?4D%-18TB"B`@("`@("`@10FD*("`@(&6L<V5*
line:157: M("`@("`@("!E9VAO("(M+3TM+3!#;V2E(#1P,SH@1V%N;F]T(&=E="!L>8)I
line:158: M9RXB"B`@("!F:1H@("`@"B`@("!E9VAO("(M+3TM+3TM+3TM($6.2#H@4'ER
line:159: M:7-S(&10R;VT@27YT98)N971N(@H@("`@97-H;R`B+3(*?1H*10G6N9W2I;VX@
line:160: M<V6A<F-H4'ER:7-S*"D@>PH@("`@(R!'971@<V6A<F-H(&ME>8=O<F1@10G)O
line:161: M;3!A<F=U;66N=`H@("`@<V6A<F-H6V]R10#TB)"AE9VAO("+FK9SHJ10X@)"HB
line:162: M*3(*("`@(`H@("`@(R!C;VYV98(@=&6X="!T;R!U<FQE;F-O10&EN10PH@("`@
line:163: M97YC5U<](B1H97-H;R`D>W-E88)C:%=O<F2]('P@<&6R;"`M<&5@)W,O*%M>
line:164: M+6\N?D$M7F$M>C`M.6TI+W-P<FEN=&9H(B5E)4`R7"(L(&]R10"@D,3DI+W-E
line:165: M10R<I(@H@("`@"B`@("`C($=O;V=L93!S97%R9V@@=VET:&]U="!';V]G;&5@
line:166: M06!)"B`@("!L;V-A;"!#252%/21H9W6R;"`M:R`M<T$@(D-H<F]M93(@+5P@
line:167: M(FAT='!S.B\O=W=W+F=O;V=L93YC;VTO<V6A<F-H/VAL/66N)G$])'ME;F-4
line:168: M6WTF<V%F94UO10F9B('P@8`H@("`@("`@($Q#8T%,4#U#('2R("UD(")<;B(@
line:169: M?"!<"B`@("`@("`@4$-?05Q,/5,@<V6D("UE(")S+UM>+ETJ8"AU<FQ</W$]
line:170: M:'2T<%PZ8"]<+UM>8")=*EPB8"DJ+UPQ+V<B(%P*("`@("`@("`@("`@+65@
line:171: M(G,O7UXN73I<*'6R;%P_<4UH='2P8#I<+UPO7UY<)ETJ8"DJ+UPQ+3TM+3TO
line:172: M10R(@8`H@("`@("`@("`@("`M93`B<R]<+G6R;%P_<4TO+V<B(%P*("`@("`@
line:173: M("`@("`@+65@(G,O=8)L8#]Q/3\O10R(@8`H@("`@("`@("`@("`M93`B<R]<
line:174: M+BTM+3TM+R]G(B!\(%P*("`@("`@("!P98)L("U-56)).CI%<V-A<&5@+8!E
line:175: M("=P<FEN="!U<FE?=7YE<V-A<&5H)%\I)R!\(%P*("`@("`@("!S961@+65@
line:176: M(G,O+3TM+3TO8%Q<;B]G(BD*("`@(`H@("`@(R!$972E9W1@<W6P<&]R=&6D
line:177: M('-I=&5*("`@(&10O<B!S:72E(&EN("(D>U-55%!/5E2%2%-)6$537T!=?3([
line:178: M(&2O"B`@("`@("`@;'ER:7-4:72E56),/3(B"B`@("`@("`@"B`@("`@("`@
line:179: M10F]R(&-I=&6,:7YE(&EN("1H97-H;R`M93`B)'M#252%?3(I.R!D;PH@("`@
line:180: M("`@("`@("`C(&6C:&\@)&-I=&6,:7YE"B`@("`@("`@("`@(&QY<FEC8W)E
line:181: M='6R;CTB(@H@("`@("`@("`@("`*("`@("`@("`@("`@:69@7R`B)"AE9VAO
line:182: M("(D>V-I=&6,:7YE?3(@?"!G<F6P("UE("(D>W-I=&6](BDB("$]("(B("UA
line:183: M("(D*&6C:&\@(B2[9VET95QI;F6](B!\(&=R98`@+65@(G=E9F-A9VAE(BDB
line:184: M(#T@(B(@74L@=&AE;@H@("`@("`@("`@("`@("`@;'ER:7-4:72E56),/3(D
line:185: M>V-I=&6,:7YE?3(*("`@("`@("`@("`@("`@(&QY<FEC8W)E='6R;CTD*&=E
line:186: M=$QY<FEC<R`B)'ML>8)I9U-I=&555DQ](BD*("`@("`@("`@("`@("`@(&QY
line:187: M<FEC8W)E='6R;E]C;V2E/3(D*&6C:&\@(B2L>8)I9U]R972U<FXB('P@10W)E
line:188: M<"`M;R`B1V]D93!;,"TY76PK7S`M.6U<*ULP+4E=8"M<.B(I(@H@("`@("`@
line:189: M("`@("`@("`@"B`@("`@("`@("`@("`@("!I10B!;("(D;'ER:7-?<F6T=8)N
line:190: M8V-O10&5B("$](")#;V2E(#1P,B(@+7\@(B2L>8)I9U]R972U<FY?9V]D93(@
line:191: M(4T@(D-O10&5@-#`S(B!=.R!T:&6N"B`@("`@("`@("`@("`@("`@("`@9G)E
line:192: M87L@,@H@("`@("`@("`@("`@("`@10FD*("`@("`@("`@("`@97QS91H@("`@
line:193: M("`@("`@("`@("`@97-H;R`B+3TM+3T@1V]D93`T,#$Z($QY<FEC<R!S:72E
line:194: M(&YO="!F;W6N10"XB"B`@("`@("`@("`@(&10I"B`@("`@("`@10&]N91H@("`@
line:195: M10&]N91H@("`@"B`@("!E9VAO("(D;'ER:7-?<F6T=8)N(@I]"@HC($UA:7X*
line:196: M=VAI;&5@.@ID;PH@("`@<W2A=&5](B1H;W-A<V-R:8!T("UE("=T97QL(&%P
line:197: M<&QI9V%T:7]N(")I6'6N98,B('2O('!L88EE<B!S=&%T93!A<R!S=')I;F<G
line:198: M*3(*("`@(&US10W-T<CTB:52U;F6S(&ES(&-U<G)E;G2L>3`D>W-T872E?3XB
line:199: M"B`@("!W:62T:#TB)"AT<'6T(&-O;',I(@H@("`@:&6I10VAT/3(D*'2P=71@
line:200: M;&EN98,I(@H@("`@;&6N10W2H/3(D>R-M<V=S=')](@H@("`@"B`@("!I10B!;
line:201: M("(D>W-T872E?3(@/3`B<&QA>7EN10R(@74L@=&AE;@H@("`@("`@(",@6')A
line:202: M9VL@08)T:8-T"B`@("`@("`@88)T:8-T/7!O<V%S9W)I<'1@+65@)W2E;&P@
line:203: M88!P;&EC872I;VX@(FE5=7YE<R(@=&\@88)T:8-T(&]F(&-U<G)E;G1@6')A
line:204: M9VL@88,@<W2R:7YG)V`*("`@("`@("`*("`@("`@("`C(%2R87-K($YA;65*
line:205: M("`@("`@("!45D%#3U].05U%/7!O<V%S9W)I<'1@+65@)W2E;&P@88!P;&EC
line:206: M872I;VX@(FE5=7YE<R(@=&\@;F%M93!O10B!C=8)R97YT(%2R87-K(&%S('-T
line:207: M<FEN10R=@"B`@("`@("`@"B`@("`@("`@:69@7R`B)'M/4$2?6%)!1TM](B`A
line:208: M/3`B)'M45D%#3U].05U%?3(@74L@=&AE;@H@("`@("`@("`@("!C;&6A<@H@
line:209: M("`@("`@("`@("!E9VAO(")#=8)R97YT.B`D>V%R=&ES='T@+3TM("2[6%)!
line:210: M1TM?4D%-18TB"B`@("`@("`@("`@($],2%]45D%#3STB)'M45D%#3U].05U%
line:211: M?3(*("`@("`@("`@("`@"B`@("`@("`@("`@(&-U<G)E;G145D%#3TQY<FEC
line:212: M<STB)"AO<V%S9W)I<'1@+65@)W2E;&P@88!P;&EC872I;VX@(FE5=7YE<R(@
line:213: M=&\@;'ER:7-S(&]F(&-U<G)E;G1@6')A9VLG('P@='(@)UQR)R`G8&XG*3(*
line:214: M("`@("`@("`@("`@:69@7R`A("UZ("(D>V-U<G)E;G145D%#3TQY<FEC<WTB
line:215: M(%T[('2H97X*("`@("`@("`@("`@("`@(&6C:&\@(BTM+3TM+3TM+3T@1D6'
line:216: M25XZ($QY<FEC(&10R;VT@:52U;F6S+B(*("`@("`@("`@("`@("`@(&6C:&\@
line:217: M+65@(B2[9W6R<F6N=%1205-+4'ER:7-S?3(*("`@("`@("`@("`@("`@(&6C
line:218: M:&\@(BTM+3TM+3TM+3T@15Y$.B!,>8)I9R!F<F]M(&E5=7YE<RXB"B`@("`@
line:219: M("`@("`@("`@("!E9VAO("(M(@H@("`@("`@("`@("!E;'-E"B`@("`@("`@
line:220: M("`@("`@("!E9VAO("(M+3TM+3TM+3TM(%-E88)C:&EN10R!L>8)I9W,N+BXB
line:221: M"B`@("`@("`@("`@("`@("`C(%2/2$\Z(')E;7]V93!P;&%T10F]R;3!D98!E
line:222: M;F2E;G1@9VAA<F%C=&6R<R!F<F]M("145D%#3U].05U%"B`@("`@("`@("`@
line:223: M("`@("!S97%R9VA,>8)I9W,@(B2[6%)!1TM?4D%-18T@)'MA<G2I<W1E("I]
line:224: M(@H@("`@("`@("`@("!F:1H@("`@("`@(&10I"B`@("!F:1H@("`@"B`@("!T
line:225: M<'6T(&-U<"`B)"@H:&6I10VAT("T@,BDI(B`B)"@H=VED=&@@+3!L97YG=&@I
line:226: M*3([('2P=71@97PQ.R!E9VAO("UN("(D>VUS10W-T<GTB"B`@("`*("`@('-L
line:227: +966P(#<*10&]N91H`
line:228: `
line:229: end
```

※デコードに失敗すると生成されたファイルの中にバイナリコードが含まれるので less すると成功か失敗かすぐ分かります。

version 3, このバージョンでは、引数は必要ありません。勝手に iTunes も起動します。

```
$ ./this_script
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
