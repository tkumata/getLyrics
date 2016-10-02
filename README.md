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
line:37: M93!E;F-O10&EN10PI#4TY34TQ%8T-(06)?5T55/3(D*&QO9V%L93!C:&%R;7%P
line:38: M*3(*"B,@27YI="X*4%E225-38U)%5U6,6#TB(@I/4$2?6%)!1TL](B(*"B,@
line:39: M062D:7YG(&QY<FEC('2O(&E5=7YE<RX*10G6N9W2I;VX@862D4'ER:7-S*"D@
line:40: M>PH@("`@;&]C87P@;'ER:7-S5W2R/3(D,3(*("`@(&]S88-C<FEP="`M93`G
line:41: M=&6L;"!A<'!L:7-A=&EO;B`B:52U;F6S(B!T;R!S971@;'ER:7-S(&]F(&-U
line:42: M<G)E;G1@6')A9VL@=&\@(B<B)'ML>8)I9W-4=')](B<B)PH@("`@97-H;R`B
line:43: M+3TM+3T@062D(&QY<FEC<R!T;R!I6'6N98,N(@I]"@HC($=E='2I;F<@;'ER
line:44: M:7-S+@IF=7YC=&EO;B!G972,>8)I9W,H*3!["B`@("!L;V-A;"!P56),/21Q
line:45: M"B`@("`*("`@(&EF(%L@)"AE9VAO("(D>W!55DQ](B!\(&=R98`@+65@)W6T
line:46: M83UN971G*3!=.R!T:&6N"B`@("`@("`@(R!W=W<N=72A+7YE="YC;VTO<V]N
line:47: M10PH@("`@("`@(&QO9V%L('!A10V6?10&%T84TD*&-U<FP@+8,@(B2[<%524'TB
line:48: M('P@;FMF("UW*1H@("`@("`@(&QO9V%L('-O;F=?=&ET;&5])"AE9VAO("(D
line:49: M>W!A10V6?10&%T88TB('P@8`H@("`@("`@("`@("!A=VL@)VUA=&-H*"1P+"`O
line:50: M/'2I=&QE/EM>/ETJ/%PO=&ET;&5^+RD@>W!R:7YT('-U9G-T<B@D,"P@5E-5
line:51: M06)5+"!24$6.2U2(*8TG('P@8`H@("`@("`@("`@("!A=VL@)WMG<W6B*"(\
line:52: M7UX^73H^(BP@(B(I?4$G*1H@("`@("`@(&QO9V%L(&QY<FEC8U524#TD*&6C
line:53: M:&\@(B2[<&%G96]D872A?3(@?"!G<F6P(")S:&]W:V%S:3YP:'`B('P@8`H@
line:54: M("`@("`@("`@("!A=VL@)VUA=&-H*"1P+"`O8"]U<V6R8"]P:'!L:7)<+W-V
line:55: M10UPO<VAO=VMA<VE<+G!H<%P_241]7S`M.6TK)B\I('MP<FEN="!S=7)S='(H
line:56: M)#`L(%)36$%26"P@5DQ%4D=43"TQ*8TG*1H@("`@("`@(&QO9V%L($Q95DE#
line:57: M5U]216-54%1])"AC=8)L("US(")H='2P.B\O=W=W+G6T83UN971N9V]M+R2[
line:58: M;'ER:7-?56),?3(@?"!<"B`@("`@("`@("`@('-E10"`M93`B<R]<+W2E>'1^
line:59: M/'2E>'1O8"]T98AT/EQ<8&X\=&6X="]G(B!\(%P*("`@("`@("`@("`@88=K
line:60: M("=[10W-U9B@B/%M>/ETJ/B(L("(B*8TQ)RD*"B`@("!E;&EF(%L@)"AE9VAO
line:61: M("(D>W!55DQ](B!\(&=R98`@+65@)W=W=RYK10V6T+FIP)RD@74L@=&AE;@H@
line:62: M("`@("`@(",@=W=W+FMG971N:G`*("`@("`@("!L;V-A;"!P87=E8V2A=&$]
line:63: M)"AC=8)L("US("(D>W!55DQ](B!\('2R("UD(")<<B(@?"!T<B`B8&XB(")\
line:64: M(B!\('-E10"`M93`B<R\\9G(@8"\^+R]G(BD*("`@("`@("!L;V-A;"!S;VYG
line:65: M8W2I=&QE/21H97-H;R`B)'MP87=E8V2A=&%](B!\(%P*("`@("`@("`@("`@
line:66: M88=K("=M872C:"@D,"P@+SQT:72L94Y;8CY=*CQ<+W2I=&QE/B\I('MP<FEN
line:67: M="!S=7)S='(H)#`L(%)36$%26"P@5DQ%4D=43"E])R!\(%P*("`@("`@("`@
line:68: M("`@88=K("=[10W-U9B@B/%M>/ETJ/B(L("(B*8TQ)RD*("`@("`@("!L;V-A
line:69: M;"!,66))1U-?5D5355Q5/21H97-H;R`B)'MP87=E8V2A=&%](B!\(%P*("`@
line:70: M("`@("`@("`@88=K("=M872C:"@D,"P@+SQD:79@:61])UPB)VQY<FEC+72R
line:71: M=7YK)UPB)SY;8CY=*CQ<+V2I=CXO*3![<')I;G1@<W6B<W2R*"1P+"!25U2!
line:72: M5E1L(%),15Y'6$@I?3<@?"!<"B`@("`@("`@("`@(&%W:R`G>V=S=7(H(CQ;
line:73: M8CY=*CXB+"`B(BE],3<@?"!<"B`@("`@("`@("`@('-E10"`M93`B<R]\+UQ<
line:74: M8&XO10R(@?"!<"B`@("`@("`@("`@('!E<FP@+5U(6$U,.CI%;G2I=&EE<R`M
line:75: M93`G=VAI;&5H/#XI('MB:7YM;V2E(%-42$]56"PB.F6N9V]D:7YG*"<D1T].
line:76: M5T],16]#3$%28U-%6"<I(CL@<')I;G1@10&6C;V2E8V6N=&ET:66S*"2?*4M]
line:77: M)RD*"B`@("!E;&EF(%L@)"AE9VAO("(D>W!55DQ](B!\(&=R98`@+65@)VMA
line:78: M<VDM=&EM93<I(%T[('2H97X*("`@("`@("`C('=W=RYK88-I+72I;65N9V]M
line:79: M"B`@("`@("`@;&]C87P@<&%G96]D872A/21H9W6R;"`M<R`B)'MP56),?3(@
line:80: M?"!T<B`M10"`B8'(B*1H@("`@("`@(&QO9V%L('2M<%]S;VYG8W2I=&QE/21H
line:81: M97-H;R`B)'MP87=E8V2A=&%](B!\(%P*("`@("`@("`@("`@88=K("=M872C
line:82: M:"@D,"P@+SQT:72L94Y;8CY=*CQ<+W2I=&QE/B\I('MP<FEN="!S=7)S='(H
line:83: M)#`L(%)36$%26"P@5DQ%4D=43"E])R!\(%P*("`@("`@("`@("`@88=K("=[
line:84: M10W-U9B@B/%M>/ETJ/B(L("(B*8TQ)RD*("`@("`@("!L;V-A;"!S;VYG8W2I
line:85: M=&QE/3(D>W2M<%]S;VYG8W2I=&QE)3`M("I](@H@("`@("`@(&QO9V%L($Q10
line:86: M5DE#5U]216-54%1])"AE9VAO("(D>W!A10V6?10&%T88TB('P@8`H@("`@("`@
line:87: M("`@("!A=VL@)VUA=&-H*"1P+"`O=F%R(&QY<FEC<R`]+BHO*3![<')I;G1@
line:88: M<W6B<W2R*"1P+"!25U2!5E1L(%),15Y'6$@I?3<@?"!<"B`@("`@("`@("`@
line:89: M('-E10"`M93`B<R]V88(@;'ER:7-S(#T@8"<O+R(@8`H@("`@("`@("`@("`@
line:90: M("`@+65@(G,O8"<[+R\B(%P*("`@("`@("`@("`@("`@("UE(")S?#QB<CY\
line:91: M8%Q<;GQG(B!\(%P*("`@("`@("`@("`@<&6R;"`M35A435PZ.D6N=&ET:66S
line:92: M("UE("=W:&EL93@\/BD@>V)I;FUO10&5@5U2$4U55+"(Z97YC;V2I;F<H)R2#
line:93: M4TY34TQ%8T-(06)?5T55)RDB.R!P<FEN="!D97-O10&6?97YT:72I98,H)%\I
line:94: M.WTG*1H*("`@(&6L:69@7R`D*&6C:&\@(B2[<%524'TB('P@10W)E<"`M93`G
line:95: M;'ER:7-A;"UN;VYS97YS93<I(%T[('2H97X*("`@("`@("`C('=W=RYL>8)I
line:96: M9V%L+7YO;G-E;G-E+F-O;1H@("`@("`@(&QO9V%L('!A10V6?10&%T84TD*&-U
line:97: M<FP@+8,@(B2[<%524'TB('P@='(@+61@(EQR(B!\('2R(")<;B(@(GPB*1H@
line:98: M("`@("`@(&QO9V%L('-O;F=?=&ET;&5])"AE9VAO("(D>W!A10V6?10&%T88TB
line:99: M('P@8`H@("`@("`@("`@("!A=VL@)VUA=&-H*"1P+"`O/'2I=&QE/EM>/ETJ
line:100: M/%PO=&ET;&5^+RD@>W!R:7YT('-U9G-T<B@D,"P@5E-406)5+"!24$6.2U2(
line:101: M*8TG('P@8`H@("`@("`@("`@("!A=VL@)WMG<W6B*"(\7UX^73H^(BP@(B(I
line:102: M?4$G*1H@("`@("`@(&QO9V%L($Q95DE#5U]216-54%1])"AE9VAO("(D>W!A
line:103: M10V6?10&%T88TB('P@8`H@("`@("`@("`@("!A=VL@)VUA=&-H*"1P+"`O/&2I
line:104: M=B!C;&%S<STB9V]N=&6N="(@:61](DIA<&%N98-E(CY;8BY=*CQ<+V2I=CXO
line:105: M*3![<')I;G1@<W6B<W2R*"1P+"!25U2!5E1L(%),15Y'6$@I?3<@?"!<"B`@
line:106: M("`@("`@("`@(&%W:R`G>V=S=7(H(CQ;8CY=*CXB+"`B(BE],3<@?"!<"B`@
line:107: M("`@("`@("`@('-E10"`M93`B<R]\+UQ<8&XO10R(I"B`@("`@("`@("`@(`H@
line:108: M("`@97QI10B!;("1H97-H;R`B)'MP56),?3(@?"!G<F6P("UE("=J+7QY<FEC
line:109: M+FYE="<I(%T[('2H97X*("`@("`@("`C(&HM;'ER:7,N;F6T"B`@("`@("`@
line:110: M;&]C87P@<&%G96]D872A/21H9W6R;"`M<R`B)'MP56),?3(@?"!T<B`M10"`B
line:111: M8'(B('P@='(@(EQN(B`B?"(@?"!S961@+65@(G,O/&)R(%PO/B\O10R(I"B`@
line:112: M("`@("`@;&]C87P@<V]N10U]T:72L94TD*&6C:&\@(B2[<&%G96]D872A?3(@
line:113: M?"!<"B`@("`@("`@("`@(&%W:R`G;7%T9V@H)#`L("\\=&ET;&5^7UX^73H\
line:114: M8"]T:72L94XO*3![<')I;G1@<W6B<W2R*"1P+"!25U2!5E1L(%),15Y'6$@I
line:115: M?3<@?"!<"B`@("`@("`@("`@(&%W:R`G>V=S=7(H(CQ;8CY=*CXB+"`B(BE]
line:116: M,3<I"B`@("`@("`@;&]C87P@4%E225-38U)%5U6,6#TD*&6C:&\@(B2[<&%G
line:117: M96]D872A?3(@?"!<"B`@("`@("`@("`@(&%W:R`G;7%T9V@H)#`L("\\<"!I
line:118: M10#TG8"<G;'ER:7-";V2Y)UPG)SY;8CY=*CQ<+W`^+RD@>W!R:7YT('-U9G-T
line:119: M<B@D,"P@5E-406)5+"!24$6.2U2(*8TG('P@8`H@("`@("`@("`@("!A=VL@
line:120: M)WMG<W6B*"(\7UX^73H^(BP@(B(I?4$G('P@8`H@("`@("`@("`@("!S961@
line:121: M+65@(G,O?"]<8%QN+V<B*1H*("`@(&6L:69@7R`D*&6C:&\@(B2[<%524'TB
line:122: M('P@10W)E<"`M93`G=72A;7%P)RD@74L@=&AE;@H@("`@("`@(",@=W=W+G6T
line:123: M87UA<"YC;VT*("`@("`@("!L;V-A;"!S=8)L/21H97-H;R`B)'MP56),?3(@
line:124: M?"!S961@+65@(G,O+BIS=8)L/6PH7S`M.7$M>D$M7BU=*EPI+UPQ+V<B*1H@
line:125: M("`@("`@(&QO9V%L('!A10V6?10&%T84TD*&-U<FP@+7L@+8-!(")I5&AO;F5B
line:126: M("U,("(D>W!55DQ](B!\(&YK10B`M=R!\('2R("UD(")<;B(I"B`@("`@("`@
line:127: M;&]C87P@=&UP8W-O;F=?=&ET;&5])"AE9VAO("(D>W!A10V6?10&%T88TB('P@
line:128: M8`H@("`@("`@("`@("!A=VL@)VUA=&-H*"1P+"`O/%2)6$Q%/EM>/ETJ/%PO
line:129: M6$E44$5^+RD@>W!R:7YT('-U9G-T<B@D,"P@5E-406)5+"!24$6.2U2(*8TG
line:130: M('P@8`H@("`@("`@("`@("!A=VL@)WMG<W6B*"(\7UX^73H^(BP@(B(I?4$G
line:131: M*1H@("`@("`@(&QO9V%L('-O;F=?=&ET;&5](B2[=&UP8W-O;F=?=&ET;&5E
line:132: M+RI](@H@("`@("`@(&QO9V%L('2M<#TD*&-U<FP@+7L@+8,@+65@(B2[<%53
line:133: M4'TB("U!(")I5&AO;F5B("U,(")H='2P.B\O=W=W+G6T87UA<"YC;VTO:G-?
line:134: M<VUT+G!H<#]U;G6M/22[<W6R;'TB('P@;FMF("UW('P@8`H@("`@("`@("`@
line:135: M("!A=VL@)VUA=&-H*"1P+"`O9V]N=&6X=#(N10FEL;%2E>'2;8CY=*F2O9W6M
line:136: M97YT+G=R:72E+RD@>W!R:7YT('-U9G-T<B@D,"P@5E-406)5+"!24$6.2U2(
line:137: M*8TG('P@8`H@("`@("`@("`@("!S961@+65@(G,O.R]<8%QN+V<B*1H@("`@
line:138: M("`@(&QO9V%L($Q95DE#5U]216-54%1])"AE9VAO("UE("(D=&UP(B!\('-E
line:139: M10"`M93`B<R]<8%PG+U\O10R(@?"!<"B`@("`@("`@("`@(&%W:R`G;7%T9V@H
line:140: M)#`L("\G8"<G7UXG8"<G73HG8"<G+RD@>W!R:7YT('-U9G-T<B@D,"P@5E-5
line:141: M06)5+"!24$6.2U2(*8TG('P@8`H@("`@("`@("`@("!S961@+65@(G,O8B<O
line:142: M+R(@+65@(G,O)R1O+R(I"B`@("!F:1H@("`@"B`@("!E9VAO("(M+3TM+3TM
line:143: M+3TM($)%2TE..B!,>8)I9W,@10G)O;3!);G2E<FYE="XB"B`@("`*("`@(&EF
line:144: M(%L@(3`M>B`B)'M,66))1U-?5D5355Q5?3(@74L@=&AE;@H@("`@("`@(&6C
line:145: M:&\@+65@(B2[4%E225-38U)%5U6,6'TB"B`@("`@("`@"B`@("`@("`@(R!5
line:146: M4T2/.B!R97UO=F5@<&QA=&10O<FT@10&6P97YD97YT(&-H88)A9W2E<G,@10G)O
line:147: M;3`D6%)!1TM?4D%-11H@("`@("`@(",@:69@97-H;R`B)'MS;VYG8W2I=&QE
line:148: M?3(@?"!G<F6P("UI("(D>U1205-+8TY!346](B`^("]D979O;G6L;"`R/B9Q
line:149: M.R!T:&6N"B`@("`@("`@;&]C87P@84U@97-H;R`B)'MS;VYG8W2I=&QE?3(@
line:150: M?"!P98)L("UN93`B<')I;G1@:69@*"\D>U1205-+8TY!346]+RDB9`H@("`@
line:151: M("`@(&EF(%L@(3`M>B`B)&$B(%T[('2H97X*("`@("`@("`@("`@97-H;R`B
line:152: M+3TM+3T@6V6B('2I=&QE(&-O;G2A:7YS(%1205-+8TY!345N(@H@("`@("`@
line:153: M("`@("!A10&2,>8)I9W,@(B2[4%E225-38U)%5U6,6'TB"B`@("`@("`@97QS
line:154: M91H@("`@("`@("`@("!E9VAO("(M+3TM+3!#;V2E(#1P,CH@6&ET;&5@10&EF
line:155: M10F6R<RXB"B`@("`@("`@("`@(&6C:&\@(E-I=&5@=&ET;&5Z("`@("2[<V]N
line:156: M10U]T:72L98TB"B`@("`@("`@("`@(&6C:&\@(D-U<G)E;G1@6%)!1TLZ("2[
line:157: M6%)!1TM?4D%-18TB"B`@("`@("`@10FD*("`@(&6L<V5*("`@("`@("!E9VAO
line:158: M("(M+3TM+3!#;V2E(#1P,SH@1V%N;F]T(&=E="!L>8)I9W,N(@H@("`@10FD*
line:159: M("`@(`H@("`@97-H;R`B+3TM+3TM+3TM+3!%4D1Z($QY<FEC<R!F<F]M($EN
line:160: M=&6R;F6T+B(*("`@(&6C:&\@(BTB"GT*"F10U;F-T:7]N('-E88)C:$QY<FEC
line:161: M<R@I('L*("`@(",@2V6T('-E88)C:"!K98EW;W)D(&10R;VT@88)G=7UE;G1*
line:162: M("`@('-E88)C:%]W;W)D/3(D*&6C:&\@(N:MC.BIGB`D*B(I(@H@("`@"B`@
line:163: M("`C(&-O;G10E<B!T98AT('2O('6R;&6N9V]D:7YG"B`@("!E;F-O10&6D8W-E
line:164: M88)C:%]W;W)D/3(D*&6C:&\@)'MS97%R9VA?=V]R10'T@?"!P98)L("UP93`G
line:165: M<R\H7UXM8RY^03U:83UZ,"TY73DO<W!R:7YT10B@B)25E,#)9(BP@;W)D*"1Q
line:166: M*3DO<V6G)RDB"B`@("`*("`@(",@2V]O10VQE('-E88)C:"!W:72H;W6T($=O
line:167: M;V=L93!!5$D*("`@(&QO9V%L($-)6$5])"AC=8)L("UK("US03`B1VAR;VUE
line:168: M(B`M4"`B:'2T<',Z+R]W=W<N10V]O10VQE+F-O;3]S97%R9V@_:&P]97XF<4TD
line:169: M>V6N9V]D962?<V6A<F-H8W=O<F2])G-A10F5];V10F(B!\(%P*("`@("`@("!,
line:170: M1U]!4$P]1R!T<B`M10"`B8&XB('P@8`H@("`@("`@($Q#8T%,4#U#('-E10"`M
line:171: M93`B<R];8BY=*EPH=8)L8#]Q/7AT='!<.EPO8"];8EPB73I<(EPI*B]<,3]G
line:172: M(B!<"B`@("`@("`@("`@("UE(")S+UM>+ETJ8"AU<FQ</W$]:'2T<%PZ8"]<
line:173: M+UM>8"10=*EPI*B]<,3TM+3TM+V<B(%P*("`@("`@("`@("`@+65@(G,O8"YU
line:174: M<FQ</W$]+R]G(B!<"B`@("`@("`@("`@("UE(")S+W6R;%P_<4TO+V<B(%P*
line:175: M("`@("`@("`@("`@+65@(G,O8"XM+3TM+3\O10R(@?"!<"B`@("`@("`@<&6R
line:176: M;"`M355224HZ18-C88!E("UP93`G<')I;G1@=8)I8W6N98-C88!E*"2?*3<@
line:177: M?"!<"B`@("`@("`@<V6D("UE(")S+RTM+3TM+UQ<8&XO10R(I"B`@("`*("`@
line:178: M(",@2&6T97-T('-U<'!O<G2E10"!S:72E"B`@("!F;W(@9VET96]L:7YE(&EN
line:179: M("1H97-H;R`M93`B)'M#252%?3(I.R!D;PH@("`@("`@(&EF(%L@(B1H97-H
line:180: M;R`B)'MC:72E8VQI;F6](B!\(&=R98`@+65@(G=E9F-A9VAE(BDB(#T@(B(@
line:181: M74L@=&AE;@H@("`@("`@("`@("!F;W(@<VET93!I;B`B)'M356!04U)41414
line:182: M252%5UM`78TB.R!D;PH@("`@("`@("`@("`@("`@(R!E9VAO("2C:72E8VQI
line:183: M;F5*("`@("`@("`@("`@("`@(&QY<FEC<U]S:72E8U524#TB(@H@("`@("`@
line:184: M("`@("`@("`@;'ER:7-S8W)E='6R;CTB(@H@("`@("`@("`@("`@("`@"B`@
line:185: M("`@("`@("`@("`@("!I10B!;("(D*&6C:&\@(B2[9VET96]L:7YE?3(@?"!G
line:186: M<F6P("UE("(D>W-I=&6](BDB("$]("(B(%T[('2H97X*("`@("`@("`@("`@
line:187: M("`@("`@("!L>8)I9W-?<VET96]55DP](B2[9VET96]L:7YE?3(*("`@("`@
line:188: M("`@("`@("`@("`@("!L>8)I9W-?<F6T=8)N/21H10V6T4'ER:7-S("(D>VQY
line:189: M<FEC<U]S:72E8U524'TB*1H@("`@("`@("`@("`@("`@("`@(&QY<FEC<U]R
line:190: M972U<FY?9V]D94TB)"AE9VAO("(D;'ER:7-S8W)E='6R;B(@?"!G<F6P("UO
line:191: M(")#;V2E(%LP+4E=8"M;,"TY76PK7S`M.6U<*UPZ(BDB"B`@("`@("`@("`@
line:192: M("`@("`@("`@"B`@("`@("`@("`@("`@("`@("`@:69@7R`B)&QY<FEC<U]R
line:193: M972U<FY?9V]D93(@(4T@(D-O10&5@-#`R(B`M;R`B)&QY<FEC<U]R972U<FY?
line:194: M9V]D93(@(4T@(D-O10&5@-#`S(B!=.R!T:&6N"B`@("`@("`@("`@("`@("`@
line:195: M("`@("`@(&)R97%K(#(*("`@("`@("`@("`@("`@("`@("!F:1H@("`@("`@
line:196: M("`@("`@("`@(R!E;'-E"B`@("`@("`@("`@("`@("`C("`@("!E9VAO("(M
line:197: M+3TM+3!#;V2E(#1P,4H@4'ER:7-S('-I=&5@;F]T(&10O=7YD+B(*("`@("`@
line:198: M("`@("`@("`@(&10I"B`@("`@("`@("`@(&2O;F5*("`@("`@("!F:1H@("`@
line:199: M10&]N91H@("`@"B`@("!E9VAO("(D;'ER:7-S8W)E='6R;B(*?1H*(R!-87EN
line:200: M"G=H:7QE(#H*10&\*("`@('-T872E/3(D*&]S88-C<FEP="`M93`G=&6L;"!A
line:201: M<'!L:7-A=&EO;B`B:52U;F6S(B!T;R!P;&%Y98(@<W2A=&5@88,@<W2R:7YG
line:202: M)RDB"B`@("!M<V=S='(](FE5=7YE<R!I<R!C=8)R97YT;'D@)'MS=&%T98TN
line:203: M(@H@("`@=VED=&@](B1H='!U="!C;VQS*3(*("`@(&AE:7=H=#TB)"AT<'6T
line:204: M(&QI;F6S*3(*("`@(&QE;F=T:#TB)'LC;8-G<W2R?3(*("`@(`H@("`@:69@
line:205: M7R`B)'MS=&%T98TB(#T@(G!L88EI;F<B(%T[('2H97X*("`@("`@("`C(%2R
line:206: M87-K($%R=&ES=`H@("`@("`@(&%R=&ES=#U@;W-A<V-R:8!T("UE("=T97QL
line:207: M(&%P<&QI9V%T:7]N(")I6'6N98,B('2O(&%R=&ES="!O10B!C=8)R97YT(%2R
line:208: M87-K(&%S('-T<FEN10R=@"B`@("`@("`@"B`@("`@("`@(R!5<F%C:R!.87UE
line:209: M"B`@("`@("`@6%)!1TM?4D%-14U@;W-A<V-R:8!T("UE("=T97QL(&%P<&QI
line:210: M9V%T:7]N(")I6'6N98,B('2O(&YA;65@;V9@9W6R<F6N="!5<F%C:R!A<R!S
line:211: M=')I;F<G9`H@("`@("`@(`H@("`@("`@(&EF(%L@(B2[4TQ$8U1205-+?3(@
line:212: M(4T@(B2[6%)!1TM?4D%-18TB(%T[('2H97X*("`@("`@("`@("`@9VQE88(*
line:213: M("`@("`@("`@("`@97-H;R`B1W6R<F6N=#H@)'MA<G2I<W2]("TM+3`D>U13
line:214: M05-+8TY!346](@H@("`@("`@("`@("!/4$2?6%)!1TL](B2[6%)!1TM?4D%-
line:215: M18TB"B`@("`@("`@("`@(`H@("`@("`@("`@("!C=8)R97YT6%)!1TM,>8)I
line:216: M9W,](B1H;W-A<V-R:8!T("UE("=T97QL(&%P<&QI9V%T:7]N(")I6'6N98,B
line:217: M('2O(&QY<FEC<R!O10B!C=8)R97YT(%2R87-K)R!\('2R("=<<B<@)UQN)RDB
line:218: M"B`@("`@("`@("`@(&EF(%L@(3`M>B`B)'MC=8)R97YT6%)!1TM,>8)I9W-]
line:219: M(B!=.R!T:&6N"B`@("`@("`@("`@("`@("!E9VAO("(M+3TM+3TM+3TM($)%
line:220: M2TE..B!,>8)I9W,@10G)O;3!I6'6N98,N(@H@("`@("`@("`@("`@("`@97-H
line:221: M;R`M93`B)'MC=8)R97YT6%)!1TM,>8)I9W-](@H@("`@("`@("`@("`@("`@
line:222: M97-H;R`B+3TM+3TM+3TM+3!%4D1Z($QY<FEC<R!F<F]M(&E5=7YE<RXB"B`@
line:223: M("`@("`@("`@("`@("!E9VAO("(M(@H@("`@("`@("`@("!E;'-E"B`@("`@
line:224: M("`@("`@("`@("!E9VAO("(M+3TM+3TM+3TM(%-E88)C:&EN10R!L>8)I9W,N
line:225: M+BXB"B`@("`@("`@("`@("`@("`C(%2/2$\Z(')E;7]V93!P;&%T10F]R;3!D
line:226: M98!E;F2E;G1@9VAA<F%C=&6R<R!F<F]M("145D%#3U].05U%"B`@("`@("`@
line:227: M("`@("`@("!S97%R9VA,>8)I9W,@(B2[6%)!1TM?4D%-18T@)'MA<G2I<W1E
line:228: M("I](@H@("`@("`@("`@("!F:1H@("`@("`@(&10I"B`@("!F:1H@("`@"B`@
line:229: M("!T<'6T(&-U<"`B)"@H:&6I10VAT("T@,BDI(B`B)"@H=VED=&@@+3!L97YG
line:230: M=&@I*3([('2P=71@97PQ.R!E9VAO("UN("(D>VUS10W-T<GTB"B`@("`*("`@
line:231: .('-L966P(#<*10&]N91H`
line:232: `
line:233: end
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
