# バシュっと歌詞のテキストデータを取得できる bash スクリプト

## お知らせ

問 3 と 4 を追加しました。問 3 は ver 3 になり新しい機能や対応サイトを追加しました。詳細は下記仕様をご覧下さい。なお、問 3 は簡単なひっかけ問題になっています。


## 概要

これは、いくつかの歌詞表示サイトからテキストな歌詞データを取得する bash スクリプトです。スクリプトは PNG 画像に埋め込んだり、文字列にエンコードしたりしています。これらはワンライナで取り出すことができます。暇だったら挑戦してみてください。取り出したスクリプトにはド定番のコードからこの用途でしか使わない正規表現が入っているので、シェルおじさんになりたいマンやシェルに興味ありウーマンの方々が挑戦してくれると嬉しいです。

なお、現状洋楽には弱いです。壊滅的です。今後対応します。対応サイトも増やす予定です。

!["ver 2 スクショ"](./ScreenShot.png)

埋め込んであるスクリプト(ver 2)は GNU 系を回避したはずなので、UNIX / Unix like で動くと思います。当方 macOS Sierra と Raspbian jessie PIXEL と Bash on Ubuntu on Windows で動作確認しています。


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
5. Require nkf libhtml-parser-perl liburi-escape-perl

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
line:27: M"G-E="`M975*9VQE88(*"FME<FY?;F%M94TB)"AU;F%M93`M83!\(&%W:R`G
line:28: M>W!R:7YT("1Q?3<I(@H*:69@7R`B)&ME<FY?;F%M93(@(4T@(D2A<G=I;B(@
line:29: M74L@=&AE;@H@("`@97-H;R`B5&QE88-E(')U;B!O;B!/5R!9+B(*("`@(&6X
line:30: M:71@,1IF:1H*(R!3971@<VET93!W:&EC:"!S=8!P;W)T(&QY<FEC<PI356!1
line:31: M4U)41413252%5STH(G=W=RYK88-I+72I;65N9V]M+VET97TB(")J+7QY<FEC
line:32: M+FYE="(@(G=W=RYK10V6T+FIP+VQY<FEC(B`B=W=W+G6T83UN971N9V]M+W-O
line:33: M;F<B(")W=W<N=72A;7%P+F-O;3(@(G=W=RYL>8)I9V%L+7YO;G-E;G-E+F-O
line:34: M;3(I"@HC($=E="!C;VYS;VQE(&6N9V]D:7YG"D-/4E-/4$6?1TA!5E]3151]
line:35: M(B1H;&]C87QE(&-H88)M88`I(@H*(R!);FET+@I,66))1U-?5D5355Q5/3(B
line:36: M"D],2%]45D%#3STB(@H*(R!!10&2I;F<@;'ER:7,@=&\@:52U;F6S+@IF=7YC
line:37: M=&EO;B!A10&2,>8)I9W,H*3!["B`@("!L;V-A;"!L>8)I9W-4='(](B1Q(@H@
line:38: M("`@;W-A<V-R:8!T("UE("=T97QL(&%P<&QI9V%T:7]N(")I6'6N98,B('2O
line:39: M('-E="!L>8)I9W,@;V9@9W6R<F6N="!5<F%C:R!T;R`B)R(D>VQY<FEC<U-T
line:40: M<GTB)R(G"B`@("!E9VAO("(M+3TM+3!!10&1@;'ER:7-S('2O(&E5=7YE<RXB
line:41: M"GT*"B,@2V6T=&EN10R!L>8)I9W,N"F10U;F-T:7]N(&=E=$QY<FEC<R@I('L*
line:42: M("`@(&QO9V%L('!55DP](B1Q(@H*("`@(&EF(%L@)"AE9VAO("(D>W!55DQ]
line:43: M(B!\(&=R98`@+65@)W6T83UN971G*3!=.R!T:&6N"B`@("`@("`@(R!W=W<N
line:44: M=72A+7YE="YC;VTO<V]N10PH@("`@("`@(&QO9V%L('!A10V6?10&%T84TD*&-U
line:45: M<FP@+8,@(B2[<%524'TB('P@;FMF("UW*1H@("`@("`@(&QO9V%L('-O;F=?
line:46: M=&ET;&5])"AE9VAO("(D>W!A10V6?10&%T88TB('P@8`H@("`@("`@("`@("!A
line:47: M=VL@)VUA=&-H*"1P+"`O/'2I=&QE/EM>/ETJ/%PO=&ET;&5^+RD@>W!R:7YT
line:48: M('-U9G-T<B@D,"P@5E-406)5+"!24$6.2U2(*8TG('P@8`H@("`@("`@("`@
line:49: M("!A=VL@)WMG<W6B*"(\7UX^73H^(BP@(B(I?4$G*1H@("`@("`@(&QO9V%L
line:50: M(&QY<FEC8U524#TD*&6C:&\@(B2[<&%G96]D872A?3(@?"!G<F6P(")S:&]W
line:51: M:V%S:3YP:'`B('P@8`H@("`@("`@("`@("!A=VL@)VUA=&-H*"1P+"`O8"]U
line:52: M<V6R8"]P:'!L:7)<+W-V10UPO<VAO=VMA<VE<+G!H<%P_241]7S`M.6TK)B\I
line:53: M('MP<FEN="!S=7)S='(H)#`L(%)36$%26"P@5DQ%4D=43"TQ*8TG*1H@("`@
line:54: M("`@(&QO9V%L($Q95DE#5U]216-54%1])"AC=8)L("US(")H='2P.B\O=W=W
line:55: M+G6T83UN971N9V]M+R2[;'ER:7-?56),?3(@?"!<"B`@("`@("`@("`@('-E
line:56: M10"`M93`B<R]<+W2E>'1^/'2E>'1O8"]T98AT/EQ<8&X\=&6X="]G(B!\(%P*
line:57: M("`@("`@("`@("`@88=K("=[10W-U9B@B/%M>/ETJ/B(L("(B*8TQ)RD*"B`@
line:58: M("!E;&EF(%L@)"AE9VAO("(D>W!55DQ](B!\(&=R98`@+65@)W=W=RYK10V6T
line:59: M+FIP)RD@74L@=&AE;@H@("`@("`@(",@=W=W+FMG971N:G`*("`@("`@("!L
line:60: M;V-A;"!P87=E8V2A=&$])"AC=8)L("US("(D>W!55DQ](B!\('2R("UD(")<
line:61: M<B(@?"!T<B`B8&XB(")\(B!\('-E10"`M93`B<R\\9G(@8"\^+R]G(BD*("`@
line:62: M("`@("!L;V-A;"!S;VYG8W2I=&QE/21H97-H;R`B)'MP87=E8V2A=&%](B!\
line:63: M(%P*("`@("`@("`@("`@88=K("=M872C:"@D,"P@+SQT:72L94Y;8CY=*CQ<
line:64: M+W2I=&QE/B\I('MP<FEN="!S=7)S='(H)#`L(%)36$%26"P@5DQ%4D=43"E]
line:65: M)R!\(%P*("`@("`@("`@("`@88=K("=[10W-U9B@B/%M>/ETJ/B(L("(B*8TQ
line:66: M)RD*("`@("`@("!L;V-A;"!,66))1U-?5D5355Q5/21H97-H;R`B)'MP87=E
line:67: M8V2A=&%](B!\(%P*("`@("`@("`@("`@88=K("=M872C:"@D,"P@+SQD:79@
line:68: M:61])UPB)VQY<FEC+72R=7YK)UPB)SY;8CY=*CQ<+V2I=CXO*3![<')I;G1@
line:69: M<W6B<W2R*"1P+"!25U2!5E1L(%),15Y'6$@I?3<@?"!<"B`@("`@("`@("`@
line:70: M(&%W:R`G>V=S=7(H(CQ;8CY=*CXB+"`B(BE],3<@?"!<"B`@("`@("`@("`@
line:71: M('-E10"`M93`B<R]\+UQ<8&XO10R(@?"!<"B`@("`@("`@("`@('!E<FP@+5U(
line:72: M6$U,.CI%;G2I=&EE<R`M93`G=VAI;&5H/#XI('MB:7YM;V2E(%-42$]56"PB
line:73: M.F6N9V]D:7YG*"<D1T].5T],16]#3$%28U-%6"<I(CL@<')I;G1@10&6C;V2E
line:74: M8V6N=&ET:66S*"2?*4M])RD*"B`@("!E;&EF(%L@)"AE9VAO("(D>W!55DQ]
line:75: M(B!\(&=R98`@+65@)VMA<VDM=&EM93<I(%T[('2H97X*("`@("`@("`C('=W
line:76: M=RYK88-I+72I;65N9V]M"B`@("`@("`@;&]C87P@<&%G96]D872A/21H9W6R
line:77: M;"`M<R`B)'MP56),?3(@?"!T<B`M10"`B8'(B*1H@("`@("`@(&QO9V%L('2M
line:78: M<%]S;VYG8W2I=&QE/21H97-H;R`B)'MP87=E8V2A=&%](B!\(%P*("`@("`@
line:79: M("`@("`@88=K("=M872C:"@D,"P@+SQT:72L94Y;8CY=*CQ<+W2I=&QE/B\I
line:80: M('MP<FEN="!S=7)S='(H)#`L(%)36$%26"P@5DQ%4D=43"E])R!\(%P*("`@
line:81: M("`@("`@("`@88=K("=[10W-U9B@B/%M>/ETJ/B(L("(B*8TQ)RD*("`@("`@
line:82: M("!L;V-A;"!S;VYG8W2I=&QE/3(D>W2M<%]S;VYG8W2I=&QE)3`M("I](@H@
line:83: M("`@("`@(&QO9V%L($Q95DE#5U]216-54%1])"AE9VAO("(D>W!A10V6?10&%T
line:84: M88TB('P@8`H@("`@("`@("`@("!A=VL@)VUA=&-H*"1P+"`O=F%R(&QY<FEC
line:85: M<R`]+BHO*3![<')I;G1@<W6B<W2R*"1P+"!25U2!5E1L(%),15Y'6$@I?3<@
line:86: M?"!<"B`@("`@("`@("`@('-E10"`M93`B<R]V88(@;'ER:7-S(#T@8"<O+R(@
line:87: M8`H@("`@("`@("`@("`@("`@+65@(G,O8"<[+R\B(%P*("`@("`@("`@("`@
line:88: M("`@("UE(")S?#QB<CY\8%Q<;GQG(B!\(%P*("`@("`@("`@("`@<&6R;"`M
line:89: M35A435PZ.D6N=&ET:66S("UE("=W:&EL93@\/BD@>V)I;FUO10&5@5U2$4U55
line:90: M+"(Z97YC;V2I;F<H)R2#4TY34TQ%8T-(06)?5T55)RDB.R!P<FEN="!D97-O
line:91: M10&6?97YT:72I98,H)%\I.WTG*1H*("`@(&6L:69@7R`D*&6C:&\@(B2[<%53
line:92: M4'TB('P@10W)E<"`M93`G;'ER:7-A;"UN;VYS97YS93<I(%T[('2H97X*("`@
line:93: M("`@("`C('=W=RYL>8)I9V%L+7YO;G-E;G-E+F-O;1H@("`@("`@(&QO9V%L
line:94: M('!A10V6?10&%T84TD*&-U<FP@+8,@(B2[<%524'TB('P@='(@+61@(EQR(B!\
line:95: M('2R(")<;B(@(GPB*1H@("`@("`@(&QO9V%L('-O;F=?=&ET;&5])"AE9VAO
line:96: M("(D>W!A10V6?10&%T88TB('P@8`H@("`@("`@("`@("!A=VL@)VUA=&-H*"1P
line:97: M+"`O/'2I=&QE/EM>/ETJ/%PO=&ET;&5^+RD@>W!R:7YT('-U9G-T<B@D,"P@
line:98: M5E-406)5+"!24$6.2U2(*8TG('P@8`H@("`@("`@("`@("!A=VL@)WMG<W6B
line:99: M*"(\7UX^73H^(BP@(B(I?4$G*1H@("`@("`@(&QO9V%L($Q95DE#5U]216-6
line:100: M4%1])"AE9VAO("(D>W!A10V6?10&%T88TB('P@8`H@("`@("`@("`@("!A=VL@
line:101: M)VUA=&-H*"1P+"`O/&2I=B!C;&%S<STB9V]N=&6N="(@:61](DIA<&%N98-E
line:102: M(CY;8BY=*CQ<+V2I=CXO*3![<')I;G1@<W6B<W2R*"1P+"!25U2!5E1L(%),
line:103: M15Y'6$@I?3<@?"!<"B`@("`@("`@("`@(&%W:R`G>V=S=7(H(CQ;8CY=*CXB
line:104: M+"`B(BE],3<@?"!<"B`@("`@("`@("`@('-E10"`M93`B<R]\+UQ<8&XO10R(I
line:105: M"@H@("`@97QI10B!;("1H97-H;R`B)'MP56),?3(@?"!G<F6P("UE("=J+7QY
line:106: M<FEC+FYE="<I(%T[('2H97X*("`@("`@("`C(&HM;'ER:7,N;F6T"B`@("`@
line:107: M("`@;&]C87P@<&%G96]D872A/21H9W6R;"`M<R`B)'MP56),?3(@?"!T<B`M
line:108: M10"`B8'(B('P@='(@(EQN(B`B?"(@?"!S961@+65@(G,O/&)R(%PO/B\O10R(I
line:109: M"B`@("`@("`@;&]C87P@<V]N10U]T:72L94TD*&6C:&\@(B2[<&%G96]D872A
line:110: M?3(@?"!<"B`@("`@("`@("`@(&%W:R`G;7%T9V@H)#`L("\\=&ET;&5^7UX^
line:111: M73H\8"]T:72L94XO*3![<')I;G1@<W6B<W2R*"1P+"!25U2!5E1L(%),15Y'
line:112: M6$@I?3<@?"!<"B`@("`@("`@("`@(&%W:R`G>V=S=7(H(CQ;8CY=*CXB+"`B
line:113: M(BE],3<I"B`@("`@("`@;&]C87P@4%E225-38U)%5U6,6#TD*&6C:&\@(B2[
line:114: M<&%G96]D872A?3(@?"!<"B`@("`@("`@("`@(&%W:R`G;7%T9V@H)#`L("\\
line:115: M<"!I10#TG8"<G;'ER:7-";V2Y)UPG)SY;8CY=*CQ<+W`^+RD@>W!R:7YT('-U
line:116: M9G-T<B@D,"P@5E-406)5+"!24$6.2U2(*8TG('P@8`H@("`@("`@("`@("!A
line:117: M=VL@)WMG<W6B*"(\7UX^73H^(BP@(B(I?4$G('P@8`H@("`@("`@("`@("!S
line:118: M961@+65@(G,O?"]<8%QN+V<B*1H*("`@(&6L:69@7R`D*&6C:&\@(B2[<%53
line:119: M4'TB('P@10W)E<"`M93`G=72A;7%P)RD@74L@=&AE;@H@("`@("`@(",@=W=W
line:120: M+G6T87UA<"YC;VT*("`@("`@("!L;V-A;"!S=8)L/21H97-H;R`B)'MP56),
line:121: M?3(@?"!S961@+65@(G,O+BIS=8)L/6PH7S`M.7$M>D$M7BU=*EPI+UPQ+V<B
line:122: M*1H@("`@("`@(&QO9V%L('!A10V6?10&%T84TD*&-U<FP@+7L@+8-!(")I5&AO
line:123: M;F5B("U,("(D>W!55DQ](B!\(&YK10B`M=R!\('2R("UD(")<;B(I"B`@("`@
line:124: M("`@;&]C87P@=&UP8W-O;F=?=&ET;&5])"AE9VAO("(D>W!A10V6?10&%T88TB
line:125: M('P@8`H@("`@("`@("`@("!A=VL@)VUA=&-H*"1P+"`O/%2)6$Q%/EM>/ETJ
line:126: M/%PO6$E44$5^+RD@>W!R:7YT('-U9G-T<B@D,"P@5E-406)5+"!24$6.2U2(
line:127: M*8TG('P@8`H@("`@("`@("`@("!A=VL@)WMG<W6B*"(\7UX^73H^(BP@(B(I
line:128: M?4$G*1H@("`@("`@(&QO9V%L('-O;F=?=&ET;&5](B2[=&UP8W-O;F=?=&ET
line:129: M;&5E+RI](@H@("`@("`@(&QO9V%L('2M<#TD*&-U<FP@+7L@+8,@+65@(B2[
line:130: M<%524'TB("U!(")I5&AO;F5B("U,(")H='2P.B\O=W=W+G6T87UA<"YC;VTO
line:131: M:G-?<VUT+G!H<#]U;G6M/22[<W6R;'TB('P@;FMF("UW('P@8`H@("`@("`@
line:132: M("`@("!A=VL@)VUA=&-H*"1P+"`O9V]N=&6X=#(N10FEL;%2E>'2;8CY=*F2O
line:133: M9W6M97YT+G=R:72E+RD@>W!R:7YT('-U9G-T<B@D,"P@5E-406)5+"!24$6.
line:134: M2U2(*8TG('P@8`H@("`@("`@("`@("!S961@+65@(G,O.R]<8%QN+V<B*1H@
line:135: M("`@("`@(&QO9V%L($Q95DE#5U]216-54%1])"AE9VAO("UE("(D=&UP(B!\
line:136: M('-E10"`M93`B<R]<8%PG+U\O10R(@?"!<"B`@("`@("`@("`@(&%W:R`G;7%T
line:137: M9V@H)#`L("\G8"<G7UXG8"<G73HG8"<G+RD@>W!R:7YT('-U9G-T<B@D,"P@
line:138: M5E-406)5+"!24$6.2U2(*8TG('P@8`H@("`@("`@("`@("!S961@+65@(G,O
line:139: M8B<O+R(@+65@(G,O)R1O+R(I"B`@("!F:1H*("`@(&6C:&\@(BTM+3TM+3TM
line:140: M+3T@1D6'25XZ($QY<FEC<R!F<F]M($EN=&6R;F6T+B(*("`@(`H@("`@:69@
line:141: M7R`A("UZ("(D>TQ95DE#5U]216-54%2](B!=.R!T:&6N"B`@("`@("`@97-H
line:142: M;R`M93`B)'M,66))1U-?5D5355Q5?3(*"B`@("`@("`@(R!44T2/.B!R97UO
line:143: M=F5@<&QA=&10O<FT@10&6P97YD97YT(&-H88)A9W2E<G,@10G)O;3`D6%)!1TM?
line:144: M4D%-11H@("`@("`@(",@:69@97-H;R`B)'MS;VYG8W2I=&QE?3(@?"!G<F6P
line:145: M("UI("(D>U1205-+8TY!346](B`^("]D979O;G6L;"`R/B9Q.R!T:&6N"B`@
line:146: M("`@("`@;&]C87P@84U@97-H;R`B)'MS;VYG8W2I=&QE?3(@?"!P98)L("UN
line:147: M93`B<')I;G1@:69@*"\D>U1205-+8TY!346]+RDB9`H@("`@("`@(&EF(%L@
line:148: M(3`M>B`B)&$B(%T[('2H97X*("`@("`@("`@("`@97-H;R`B+3TM+3T@1V]D
line:149: M93`Q,#`Z(%=E9B!T:72L93!C;VYT87EN<R!45D%#3U].05U%+B(*("`@("`@
line:150: M("`@("`@862D4'ER:7-S("(D>TQ95DE#5U]216-54%2](@H@("`@("`@(&6L
line:151: M<V5*("`@("`@("`@("`@97-H;R`B+3TM+3T@1V]D93`T,#(Z(%2I=&QE(&2I
line:152: M10F10E<G,N(@H@("`@("`@("`@("!E9VAO(")4:72E('2I=&QE.B`@("`D>W-O
line:153: M;F=?=&ET;&6](@H@("`@("`@("`@("!E9VAO(")#=8)R97YT(%1205-+.B`D
line:154: M>U1205-+8TY!346](@H@("`@("`@(&10I"B`@("!E;'-E"B`@("`@("`@97-H
line:155: M;R`B+3TM+3T@1V]D93`T,#,Z($-A;FYO="!G971@;'ER:7-S+B(*("`@(&10I
line:156: M"B`@("`*("`@(&6C:&\@(BTM+3TM+3TM+3T@15Y$.B!,>8)I9W,@10G)O;3!)
line:157: M;G2E<FYE="XB"B`@("!E9VAO("(M(@I]"@IF=7YC=&EO;B!S97%R9VA,>8)I
line:158: M9W,H*3!["B`@("`C($=E="!S97%R9V@@:V6Y=V]R10"!F<F]M(&%R10W6M97YT
line:159: M"B`@("!S97%R9VA?=V]R10#TD*&6C:&\@(N:MC.BIGB`D*B(I"@H@("`@(R!C
line:160: M;VYV98(@=&6X="!T;R!U<FQE;F-O10&EN10PH@("`@97YC;V2E10%]S97%R9VA?
line:161: M=V]R10#TD*&6C:&\@(B2[<V6A<F-H8W=O<F2](B!\('!E<FP@+8!E("=S+RA;
line:162: M8BU?+GY!+6IA+8HP+4E=*3]S<')I;G2F*"(E)25P,E@B+"!O<F1H)#$I*3]S
line:163: M97<G*1H*("`@(",@2V]O10VQE('-E88)C:"!W:72H;W6T($=O;V=L93!!5$D*
line:164: M("`@(&QO9V%L($-)6$5])"AC=8)L("UK("US03`B1VAR;VUE(B`M4"`B:'2T
line:165: M<',Z+R]W=W<N10V]O10VQE+F-O;3]S97%R9V@_:&P]97XF<4TD>V6N9V]D962?
line:166: M<V6A<F-H8W=O<F2])G-A10F5];V10F(B!\(%P*("`@("`@("!,1U]!4$P]1R!T
line:167: M<B`M10"`B8&XB('P@8`H@("`@("`@($Q#8T%,4#U#('-E10"`M93`B<R];8BY=
line:168: M*EPH=8)L8#]Q/7AT='!<.EPO8"];8EPB73I<(EPI*B]<,3]G(B!<"B`@("`@
line:169: M("`@("`@("UE(")S+UM>+ETJ8"AU<FQ</W$]:'2T<%PZ8"]<+UM>8"10=*EPI
line:170: M*B]<,3TM+3TM+V<B(%P*("`@("`@("`@("`@+65@(G,O8"YU<FQ</W$]+R]G
line:171: M(B!<"B`@("`@("`@("`@("UE(")S+W6R;%P_<4TO+V<B(%P*("`@("`@("`@
line:172: M("`@+65@(G,O8"XM+3TM+3\O10R(@?"!<"B`@("`@("`@<&6R;"`M355224HZ
line:173: M18-C88!E("UP93`G<')I;G1@=8)I8W6N98-C88!E*"2?*3<@?"!<"B`@("`@
line:174: M("`@<V6D("UE(")S+RTM+3TM+UQ<8&XO10R(I"@H@("`@(R!$972E9W1@<W6P
line:175: M<&]R=&6D('-I=&5*("`@(&10O<B!C:72E8VQI;F5@:7X@)"AE9VAO("UE("(D
line:176: M>T-)6$6](BD[(&2O"B`@("`@("`@:69@7R`B)"AE9VAO("(D>V-I=&6?;&EN
line:177: M98TB('P@10W)E<"`M93`B=V6B9V%C:&5B*3(@/3`B(B!=.R!T:&6N"B`@("`@
line:178: M("`@("`@(&10O<B!S:72E(&EN("(D>U-55%!/5E2%2%-)6$537T!=?3([(&2O
line:179: M"B`@("`@("`@("`@("`@("`C(&6C:&\@)&-I=&6?;&EN91H@("`@("`@("`@
line:180: M("`@("`@;'ER:7-S8W-I=&6?56),/3(B"B`@("`@("`@("`@("`@("!L>8)I
line:181: M9W-?<F6T=8)N/3(B"@H@("`@("`@("`@("`@("`@:69@7R`B)"AE9VAO("(D
line:182: M>V-I=&6?;&EN98TB('P@10W)E<"`M93`B)'MS:72E?3(I(B`A/3`B(B!=.R!T
line:183: M:&6N"B`@("`@("`@("`@("`@("`@("`@;'ER:7-S8W-I=&6?56),/3(D>V-I
line:184: M=&6?;&EN98TB"B`@("`@("`@("`@("`@("`@("`@;'ER:7-S8W)E='6R;CTD
line:185: M*&=E=$QY<FEC<R`B)'ML>8)I9W-?<VET96]55DQ](BD*("`@("`@("`@("`@
line:186: M("`@("`@("!L>8)I9W-?<F6T=8)N8V-O10&5])"AE9VAO("(D;'ER:7-S8W)E
line:187: M='6R;B(@?"!G<F6P("UO("=#;V2E(%LP+4E=*CHG*1H*("`@("`@("`@("`@
line:188: M("`@("`@("!I10B!;("(D;'ER:7-S8W)E='6R;E]C;V2E(B`A/3`B1V]D93`T
line:189: M,#(Z(B`M;R`B)&QY<FEC<U]R972U<FY?9V]D93(@(4T@(D-O10&5@-#`S.B(@
line:190: M74L@=&AE;@H@("`@("`@("`@("`@("`@("`@("`@("!B<F6A:R`R"B`@("`@
line:191: M("`@("`@("`@("`@("`@10FD*("`@("`@("`@("`@("`@(",@97QS91H@("`@
line:192: M("`@("`@("`@("`@(R`@("`@97-H;R`B+3TM+3T@1V]D93`T,#$Z($QY<FEC
line:193: M<R!S:72E(&YO="!F;W6N10"XB"B`@("`@("`@("`@("`@("!F:1H@("`@("`@
line:194: M("`@("!D;VYE"B`@("`@("`@10FD*("`@(&2O;F5*"B`@("!E9VAO("(D;'ER
line:195: M:7-S8W)E='6R;B(*?1H*(R!-87EN"G=H:7QE(#H*10&\*("`@('-T872E/3(D
line:196: M*&]S88-C<FEP="`M93`G=&6L;"!A<'!L:7-A=&EO;B`B:52U;F6S(B!T;R!P
line:197: M;&%Y98(@<W2A=&5@88,@<W2R:7YG)RDB"B`@("!M<V=S='(](FE5=7YE<R!I
line:198: M<R!C=8)R97YT;'D@)'MS=&%T98TN(@H@("`@=VED=&@](B1H='!U="!C;VQS
line:199: M*3(*("`@(&AE:7=H=#TB)"AT<'6T(&QI;F6S*3(*("`@(&QE;F=T:#TB)'LC
line:200: M;8-G<W2R?3(*"B`@("!I10B!;("(D>W-T872E?3(@/3`B<&QA>7EN10R(@74L@
line:201: M=&AE;@H@("`@("`@(",@6')A9VL@08)T:8-T"B`@("`@("`@88)T:8-T/7!O
line:202: M<V%S9W)I<'1@+65@)W2E;&P@88!P;&EC872I;VX@(FE5=7YE<R(@=&\@88)T
line:203: M:8-T(&]F(&-U<G)E;G1@6')A9VL@88,@<W2R:7YG)V`*"B`@("`@("`@(R!5
line:204: M<F%C:R!.87UE"B`@("`@("`@6%)!1TM?4D%-14U@;W-A<V-R:8!T("UE("=T
line:205: M97QL(&%P<&QI9V%T:7]N(")I6'6N98,B('2O(&YA;65@;V9@9W6R<F6N="!5
line:206: M<F%C:R!A<R!S=')I;F<G9`H@("`@("`@(`H@("`@("`@(&EF(%L@(B2[4TQ$
line:207: M8U1205-+?3(@(4T@(B2[6%)!1TM?4D%-18TB(%T[('2H97X*("`@("`@("`@
line:208: M("`@9VQE88(*("`@("`@("`@("`@97-H;R`B1W6R<F6N=#H@)'MA<G2I<W2]
line:209: M("TM+3`D>U1205-+8TY!346](@H@("`@("`@("`@("!/4$2?6%)!1TL](B2[
line:210: M6%)!1TM?4D%-18TB"@H@("`@("`@("`@("!C=8)R97YT6%)!1TM,>8)I9W,]
line:211: M(B1H;W-A<V-R:8!T("UE("=T97QL(&%P<&QI9V%T:7]N(")I6'6N98,B('2O
line:212: M(&QY<FEC<R!O10B!C=8)R97YT(%2R87-K)R!\('2R("=<<B<@)UQN)RDB"B`@
line:213: M("`@("`@("`@(&EF(%L@(3`M>B`B)'MC=8)R97YT6%)!1TM,>8)I9W-](B!=
line:214: M.R!T:&6N"B`@("`@("`@("`@("`@("!E9VAO("(M+3TM+3TM+3TM($)%2TE.
line:215: M.B!,>8)I9W,@10G)O;3!I6'6N98,N(@H@("`@("`@("`@("`@("`@97-H;R`M
line:216: M93`B)'MC=8)R97YT6%)!1TM,>8)I9W-](@H@("`@("`@("`@("`@("`@97-H
line:217: M;R`B+3TM+3TM+3TM+3!%4D1Z($QY<FEC<R!F<F]M(&E5=7YE<RXB"B`@("`@
line:218: M("`@("`@("`@("!E9VAO("(M(@H@("`@("`@("`@("!E;'-E"B`@("`@("`@
line:219: M("`@("`@("!E9VAO("(M+3TM+3TM+3TM(%-E88)C:&EN10R!L>8)I9W,N+BXB
line:220: M"B`@("`@("`@("`@("`@("`C(%2/2$\Z(')E;7]V93!P;&%T10F]R;3!D98!E
line:221: M;F2E;G1@9VAA<F%C=&6R<R!F<F]M("145D%#3U].05U%"B`@("`@("`@("`@
line:222: M("`@("!S97%R9VA,>8)I9W,@(B2[6%)!1TM?4D%-18T@)'MA<G2I<W1E("I]
line:223: M(@H@("`@("`@("`@("!F:1H@("`@("`@(&10I"B`@("!F:1H*("`@('2P=71@
line:224: M9W6P("(D*"AH97EG:'1@+3`R*3DB("(D*"AW:62T:"`M(&QE;F=T:"DI(CL@
line:225: M='!U="!E;#$[(&6C:&\@+7X@(B2[;8-G<W2R?3(*"B`@("!S;&6E<"`W"F2O
line:226: #;F5*
line:227: `
line:228: end
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
