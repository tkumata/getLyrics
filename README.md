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
line:27: M"G-E="`M975*9VQE88(*"FME<FY?;F%M94TB)"AU;F%M93`M83!\(&%W:R`G
line:28: M>W!R:7YT("1Q?3<I(@H*:69@7R`B)&ME<FY?;F%M93(@(4T@(D2A<G=I;B(@
line:29: M74L@=&AE;@H@("`@97-H;R`B5&QE88-E(')U;B!O;B!/5R!9+B(*("`@(&6X
line:30: M:71@,1IF:1H*(R!3971@<VET93!W:&EC:"!S=8!P;W)T(&QY<FEC<PI356!1
line:31: M4U)41413252%5STH(G=W=RYK88-I+72I;65N9V]M+VET97TB(")J+7QY<FEC
line:32: M+FYE="(@(G=W=RYK10V6T+FIP+VQY<FEC(B`B=W=W+G6T83UN971N9V]M+W-O
line:33: M;F<B(")W=W<N=72A;7%P+F-O;3(I"B-356!04U)41413252%5STH(G=W=RYU
line:34: M=&%M88`N9V]M(B`B=W=W+FMA<VDM=&EM93YC;VTO:72E;3(@(G=W=RYK10V6T
line:35: M+FIP+VQY<FEC(B`B:BUL>8)I9RYN971B(")W=W<N=72A+7YE="YC;VTO<V]N
line:36: M10R(@(G=W=RYL>8)I9V%L+7YO;G-E;G-E+F-O;3(I"@HC($=E="!C;VYS;VQE
line:37: M(&6N9V]D:7YG"D-/4E-/4$6?1TA!5E]3151](B1H;&]C87QE(&-H88)M88`I
line:38: M(@H*(R!);FET+@I,66))1U-?5D5355Q5/3(B"D],2%]45D%#3STB(@H*(R!!
line:39: M10&2I;F<@;'ER:7,@=&\@:52U;F6S+@IF=7YC=&EO;B!A10&2,>8)I9W,H*3![
line:40: M"B`@("!L;V-A;"!L>8)I9W-4='(](B1Q(@H@("`@;W-A<V-R:8!T("UE("=T
line:41: M97QL(&%P<&QI9V%T:7]N(")I6'6N98,B('2O('-E="!L>8)I9W,@;V9@9W6R
line:42: M<F6N="!5<F%C:R!T;R`B)R(D>VQY<FEC<U-T<GTB)R(G"B`@("!E9VAO("(M
line:43: M+3TM+3!!10&1@;'ER:7-S('2O(&E5=7YE<RXB"GT*"B,@2V6T=&EN10R!L>8)I
line:44: M9W,N"F10U;F-T:7]N(&=E=$QY<FEC<R@I('L*("`@(&QO9V%L('!55DP](B1Q
line:45: M(@H*("`@(&EF(%L@)"AE9VAO("(D>W!55DQ](B!\(&=R98`@+65@)W6T83UN
line:46: M971G*3!=.R!T:&6N"B`@("`@("`@(R!W=W<N=72A+7YE="YC;VTO<V]N10PH@
line:47: M("`@("`@(&QO9V%L('!A10V6?10&%T84TD*&-U<FP@+8,@(B2[<%524'TB('P@
line:48: M;FMF("UW*1H@("`@("`@(&QO9V%L('-O;F=?=&ET;&5])"AE9VAO("(D>W!A
line:49: M10V6?10&%T88TB('P@8`H@("`@("`@("`@("!A=VL@)VUA=&-H*"1P+"`O/'2I
line:50: M=&QE/EM>/ETJ/%PO=&ET;&5^+RD@>W!R:7YT('-U9G-T<B@D,"P@5E-406)5
line:51: M+"!24$6.2U2(*8TG('P@8`H@("`@("`@("`@("!A=VL@)WMG<W6B*"(\7UX^
line:52: M73H^(BP@(B(I?4$G*1H@("`@("`@(&QO9V%L(&QY<FEC8U524#TD*&6C:&\@
line:53: M(B2[<&%G96]D872A?3(@?"!G<F6P(")S:&]W:V%S:3YP:'`B('P@8`H@("`@
line:54: M("`@("`@("!A=VL@)VUA=&-H*"1P+"`O8"]U<V6R8"]P:'!L:7)<+W-V10UPO
line:55: M<VAO=VMA<VE<+G!H<%P_241]7S`M.6TK)B\I('MP<FEN="!S=7)S='(H)#`L
line:56: M(%)36$%26"P@5DQ%4D=43"TQ*8TG*1H@("`@("`@(&QO9V%L($Q95DE#5U]3
line:57: M16-54%1])"AC=8)L("US(")H='2P.B\O=W=W+G6T83UN971N9V]M+R2[;'ER
line:58: M:7-?56),?3(@?"!<"B`@("`@("`@("`@('-E10"`M93`B<R]<+W2E>'1^/'2E
line:59: M>'1O8"]T98AT/EQ<8&X\=&6X="]G(B!\(%P*("`@("`@("`@("`@88=K("=[
line:60: M10W-U9B@B/%M>/ETJ/B(L("(B*8TQ)RD*"B`@("!E;&EF(%L@)"AE9VAO("(D
line:61: M>W!55DQ](B!\(&=R98`@+65@)W=W=RYK10V6T+FIP)RD@74L@=&AE;@H@("`@
line:62: M("`@(",@=W=W+FMG971N:G`*("`@("`@("!L;V-A;"!P87=E8V2A=&$])"AC
line:63: M=8)L("US("(D>W!55DQ](B!\('2R("UD(")<<B(@?"!T<B`B8&XB(")\(B!\
line:64: M('-E10"`M93`B<R\\9G(@8"\^+R]G(BD*("`@("`@("!L;V-A;"!S;VYG8W2I
line:65: M=&QE/21H97-H;R`B)'MP87=E8V2A=&%](B!\(%P*("`@("`@("`@("`@88=K
line:66: M("=M872C:"@D,"P@+SQT:72L94Y;8CY=*CQ<+W2I=&QE/B\I('MP<FEN="!S
line:67: M=7)S='(H)#`L(%)36$%26"P@5DQ%4D=43"E])R!\(%P*("`@("`@("`@("`@
line:68: M88=K("=[10W-U9B@B/%M>/ETJ/B(L("(B*8TQ)RD*("`@("`@("!L;V-A;"!,
line:69: M66))1U-?5D5355Q5/21H97-H;R`B)'MP87=E8V2A=&%](B!\(%P*("`@("`@
line:70: M("`@("`@88=K("=M872C:"@D,"P@+SQD:79@:61])UPB)VQY<FEC+72R=7YK
line:71: M)UPB)SY;8CY=*CQ<+V2I=CXO*3![<')I;G1@<W6B<W2R*"1P+"!25U2!5E1L
line:72: M(%),15Y'6$@I?3<@?"!<"B`@("`@("`@("`@(&%W:R`G>V=S=7(H(CQ;8CY=
line:73: M*CXB+"`B(BE],3<@?"!<"B`@("`@("`@("`@('-E10"`M93`B<R]\+UQ<8&XO
line:74: M10R(@?"!<"B`@("`@("`@("`@('!E<FP@+5U(6$U,.CI%;G2I=&EE<R`M93`G
line:75: M=VAI;&5H/#XI('MB:7YM;V2E(%-42$]56"PB.F6N9V]D:7YG*"<D1T].5T],
line:76: M16]#3$%28U-%6"<I(CL@<')I;G1@10&6C;V2E8V6N=&ET:66S*"2?*4M])RD*
line:77: M"B`@("!E;&EF(%L@)"AE9VAO("(D>W!55DQ](B!\(&=R98`@+65@)VMA<VDM
line:78: M=&EM93<I(%T[('2H97X*("`@("`@("`C('=W=RYK88-I+72I;65N9V]M"B`@
line:79: M("`@("`@;&]C87P@<&%G96]D872A/21H9W6R;"`M<R`B)'MP56),?3(@?"!T
line:80: M<B`M10"`B8'(B*1H@("`@("`@(&QO9V%L('2M<%]S;VYG8W2I=&QE/21H97-H
line:81: M;R`B)'MP87=E8V2A=&%](B!\(%P*("`@("`@("`@("`@88=K("=M872C:"@D
line:82: M,"P@+SQT:72L94Y;8CY=*CQ<+W2I=&QE/B\I('MP<FEN="!S=7)S='(H)#`L
line:83: M(%)36$%26"P@5DQ%4D=43"E])R!\(%P*("`@("`@("`@("`@88=K("=[10W-U
line:84: M9B@B/%M>/ETJ/B(L("(B*8TQ)RD*("`@("`@("!L;V-A;"!S;VYG8W2I=&QE
line:85: M/3(D>W2M<%]S;VYG8W2I=&QE)3`M("I](@H@("`@("`@(&QO9V%L($Q95DE#
line:86: M5U]216-54%1])"AE9VAO("(D>W!A10V6?10&%T88TB('P@8`H@("`@("`@("`@
line:87: M("!A=VL@)VUA=&-H*"1P+"`O=F%R(&QY<FEC<R`]+BHO*3![<')I;G1@<W6B
line:88: M<W2R*"1P+"!25U2!5E1L(%),15Y'6$@I?3<@?"!<"B`@("`@("`@("`@('-E
line:89: M10"`M93`B<R]V88(@;'ER:7-S(#T@8"<O+R(@8`H@("`@("`@("`@("`@("`@
line:90: M+65@(G,O8"<[+R\B(%P*("`@("`@("`@("`@("`@("UE(")S?#QB<CY\8%Q<
line:91: M;GQG(B!\(%P*("`@("`@("`@("`@<&6R;"`M35A435PZ.D6N=&ET:66S("UE
line:92: M("=W:&EL93@\/BD@>V)I;FUO10&5@5U2$4U55+"(Z97YC;V2I;F<H)R2#4TY4
line:93: M4TQ%8T-(06)?5T55)RDB.R!P<FEN="!D97-O10&6?97YT:72I98,H)%\I.WTG
line:94: M*1H*("`@(&6L:69@7R`D*&6C:&\@(B2[<%524'TB('P@10W)E<"`M93`G;'ER
line:95: M:7-A;"UN;VYS97YS93<I(%T[('2H97X*("`@("`@("`C('=W=RYL>8)I9V%L
line:96: M+7YO;G-E;G-E+F-O;1H@("`@("`@(&QO9V%L('!A10V6?10&%T84TD*&-U<FP@
line:97: M+8,@(B2[<%524'TB('P@='(@+61@(EQR(B!\('2R(")<;B(@(GPB*1H@("`@
line:98: M("`@(&QO9V%L('-O;F=?=&ET;&5])"AE9VAO("(D>W!A10V6?10&%T88TB('P@
line:99: M8`H@("`@("`@("`@("!A=VL@)VUA=&-H*"1P+"`O/'2I=&QE/EM>/ETJ/%PO
line:100: M=&ET;&5^+RD@>W!R:7YT('-U9G-T<B@D,"P@5E-406)5+"!24$6.2U2(*8TG
line:101: M('P@8`H@("`@("`@("`@("!A=VL@)WMG<W6B*"(\7UX^73H^(BP@(B(I?4$G
line:102: M*1H@("`@("`@(&QO9V%L($Q95DE#5U]216-54%1])"AE9VAO("(D>W!A10V6?
line:103: M10&%T88TB('P@8`H@("`@("`@("`@("!A=VL@)VUA=&-H*"1P+"`O/&2I=B!C
line:104: M;&%S<STB9V]N=&6N="(@:61](DIA<&%N98-E(CY;8BY=*CQ<+V2I=CXO*3![
line:105: M<')I;G1@<W6B<W2R*"1P+"!25U2!5E1L(%),15Y'6$@I?3<@?"!<"B`@("`@
line:106: M("`@("`@(&%W:R`G>V=S=7(H(CQ;8CY=*CXB+"`B(BE],3<@?"!<"B`@("`@
line:107: M("`@("`@('-E10"`M93`B<R]\+UQ<8&XO10R(I"@H@("`@97QI10B!;("1H97-H
line:108: M;R`B)'MP56),?3(@?"!G<F6P("UE("=J+7QY<FEC+FYE="<I(%T[('2H97X*
line:109: M("`@("`@("`C(&HM;'ER:7,N;F6T"B`@("`@("`@;&]C87P@<&%G96]D872A
line:110: M/21H9W6R;"`M<R`B)'MP56),?3(@?"!T<B`M10"`B8'(B('P@='(@(EQN(B`B
line:111: M?"(@?"!S961@+65@(G,O/&)R(%PO/B\O10R(I"B`@("`@("`@;&]C87P@<V]N
line:112: M10U]T:72L94TD*&6C:&\@(B2[<&%G96]D872A?3(@?"!<"B`@("`@("`@("`@
line:113: M(&%W:R`G;7%T9V@H)#`L("\\=&ET;&5^7UX^73H\8"]T:72L94XO*3![<')I
line:114: M;G1@<W6B<W2R*"1P+"!25U2!5E1L(%),15Y'6$@I?3<@?"!<"B`@("`@("`@
line:115: M("`@(&%W:R`G>V=S=7(H(CQ;8CY=*CXB+"`B(BE],3<I"B`@("`@("`@;&]C
line:116: M87P@4%E225-38U)%5U6,6#TD*&6C:&\@(B2[<&%G96]D872A?3(@?"!<"B`@
line:117: M("`@("`@("`@(&%W:R`G;7%T9V@H)#`L("\\<"!I10#TG8"<G;'ER:7-";V2Y
line:118: M)UPG)SY;8CY=*CQ<+W`^+RD@>W!R:7YT('-U9G-T<B@D,"P@5E-406)5+"!3
line:119: M4$6.2U2(*8TG('P@8`H@("`@("`@("`@("!A=VL@)WMG<W6B*"(\7UX^73H^
line:120: M(BP@(B(I?4$G('P@8`H@("`@("`@("`@("!S961@+65@(G,O?"]<8%QN+V<B
line:121: M*1H*("`@(&6L:69@7R`D*&6C:&\@(B2[<%524'TB('P@10W)E<"`M93`G=72A
line:122: M;7%P)RD@74L@=&AE;@H@("`@("`@(",@=W=W+G6T87UA<"YC;VT*("`@("`@
line:123: M("!L;V-A;"!S=8)L/21H97-H;R`B)'MP56),?3(@?"!S961@+65@(G,O+BIS
line:124: M=8)L/6PH7S`M.7$M>D$M7BU=*EPI+UPQ+V<B*1H@("`@("`@(&QO9V%L('!A
line:125: M10V6?10&%T84TD*&-U<FP@+7L@+8-!(")I5&AO;F5B("U,("(D>W!55DQ](B!\
line:126: M(&YK10B`M=R!\('2R("UD(")<;B(I"B`@("`@("`@;&]C87P@=&UP8W-O;F=?
line:127: M=&ET;&5])"AE9VAO("(D>W!A10V6?10&%T88TB('P@8`H@("`@("`@("`@("!A
line:128: M=VL@)VUA=&-H*"1P+"`O/%2)6$Q%/EM>/ETJ/%PO6$E44$5^+RD@>W!R:7YT
line:129: M('-U9G-T<B@D,"P@5E-406)5+"!24$6.2U2(*8TG('P@8`H@("`@("`@("`@
line:130: M("!A=VL@)WMG<W6B*"(\7UX^73H^(BP@(B(I?4$G*1H@("`@("`@(&QO9V%L
line:131: M('-O;F=?=&ET;&5](B2[=&UP8W-O;F=?=&ET;&5E+RI](@H@("`@("`@(&QO
line:132: M9V%L('2M<#TD*&-U<FP@+7L@+8,@+65@(B2[<%524'TB("U!(")I5&AO;F5B
line:133: M("U,(")H='2P.B\O=W=W+G6T87UA<"YC;VTO:G-?<VUT+G!H<#]U;G6M/22[
line:134: M<W6R;'TB('P@;FMF("UW('P@8`H@("`@("`@("`@("!A=VL@)VUA=&-H*"1P
line:135: M+"`O9V]N=&6X=#(N10FEL;%2E>'2;8CY=*F2O9W6M97YT+G=R:72E+RD@>W!R
line:136: M:7YT('-U9G-T<B@D,"P@5E-406)5+"!24$6.2U2(*8TG('P@8`H@("`@("`@
line:137: M("`@("!S961@+65@(G,O.R]<8%QN+V<B*1H@("`@("`@(&QO9V%L($Q95DE#
line:138: M5U]216-54%1])"AE9VAO("UE("(D=&UP(B!\('-E10"`M93`B<R]<8%PG+U\O
line:139: M10R(@?"!<"B`@("`@("`@("`@(&%W:R`G;7%T9V@H)#`L("\G8"<G7UXG8"<G
line:140: M73HG8"<G+RD@>W!R:7YT('-U9G-T<B@D,"P@5E-406)5+"!24$6.2U2(*8TG
line:141: M('P@8`H@("`@("`@("`@("!S961@+65@(G,O8B<O+R(@+65@(G,O)R1O+R(I
line:142: M"B`@("!F:1H*("`@(&6C:&\@(BTM+3TM+3TM+3T@1D6'25XZ($QY<FEC<R!F
line:143: M<F]M($EN=&6R;F6T+B(*("`@(`H@("`@:69@7R`A("UZ("(D>TQ95DE#5U]3
line:144: M16-54%2](B!=.R!T:&6N"B`@("`@("`@97-H;R`M93`B)'M,66))1U-?5D54
line:145: M55Q5?3(*"B`@("`@("`@(R!44T2/.B!R97UO=F5@<&QA=&10O<FT@10&6P97YD
line:146: M97YT(&-H88)A9W2E<G,@10G)O;3`D6%)!1TM?4D%-11H@("`@("`@(",@:69@
line:147: M97-H;R`B)'MS;VYG8W2I=&QE?3(@?"!G<F6P("UI("(D>U1205-+8TY!346]
line:148: M(B`^("]D979O;G6L;"`R/B9Q.R!T:&6N"B`@("`@("`@;&]C87P@84U@97-H
line:149: M;R`B)'MS;VYG8W2I=&QE?3(@?"!P98)L("UN93`B<')I;G1@:69@*"\D>U13
line:150: M05-+8TY!346]+RDB9`H@("`@("`@(&EF(%L@(3`M>B`B)&$B(%T[('2H97X*
line:151: M("`@("`@("`@("`@97-H;R`B+3TM+3T@1V]D93`Q,#`Z(%=E9B!T:72L93!C
line:152: M;VYT87EN<R!45D%#3U].05U%+B(*("`@("`@("`@("`@862D4'ER:7-S("(D
line:153: M>TQ95DE#5U]216-54%2](@H@("`@("`@(&6L<V5*("`@("`@("`@("`@97-H
line:154: M;R`B+3TM+3T@1V]D93`T,#(Z(%2I=&QE(&2I10F10E<G,N(@H@("`@("`@("`@
line:155: M("!E9VAO(")4:72E('2I=&QE.B`@("`D>W-O;F=?=&ET;&6](@H@("`@("`@
line:156: M("`@("!E9VAO(")#=8)R97YT(%1205-+.B`D>U1205-+8TY!346](@H@("`@
line:157: M("`@(&10I"B`@("!E;'-E"B`@("`@("`@97-H;R`B+3TM+3T@1V]D93`T,#,Z
line:158: M($-A;FYO="!G971@;'ER:7-S+B(*("`@(&10I"B`@("`*("`@(&6C:&\@(BTM
line:159: M+3TM+3TM+3T@15Y$.B!,>8)I9W,@10G)O;3!);G2E<FYE="XB"B`@("!E9VAO
line:160: M("(M(@I]"@IF=7YC=&EO;B!S97%R9VA,>8)I9W,H*3!["B`@("`C($=E="!S
line:161: M97%R9V@@:V6Y=V]R10"!F<F]M(&%R10W6M97YT"B`@("!S97%R9VA?=V]R10#TB
line:162: M)"AE9VAO("+FK9SHJ10X@)"HB*3(*"B`@("`C(&-O;G10E<B!T98AT('2O('6R
line:163: M;&6N9V]D:7YG"B`@("!E;F-O10&6D8W-E88)C:%]W;W)D/3(D*&6C:&\@)'MS
line:164: M97%R9VA?=V]R10'T@?"!P98)L("UP93`G<R\H7UXM8RY^03U:83UZ,"TY73DO
line:165: M<W!R:7YT10B@B)25E,#)9(BP@;W)D*"1Q*3DO<V6G)RDB"@H@("`@(R!';V]G
line:166: M;&5@<V6A<F-H('=I=&AO=71@2V]O10VQE($%021H@("`@;&]C87P@1TE414TD
line:167: M*&-U<FP@+7L@+8-!(")#:')O;65B("U,(")H='2P<SHO+W=W=RYG;V]G;&5N
line:168: M9V]M+W-E88)C:#]H;#UE;B10Q/22[97YC;V2E10%]S97%R9VA?=V]R10'TF<V%F
line:169: M94UO10F9B('P@8`H@("`@("`@($Q#8T%,4#U#('2R("UD(")<;B(@?"!<"B`@
line:170: M("`@("`@4$-?05Q,/5,@<V6D("UE(")S+UM>+ETJ8"AU<FQ</W$]:'2T<%PZ
line:171: M8"]<+UM>8")=*EPB8"DJ+UPQ+V<B(%P*("`@("`@("`@("`@+65@(G,O7UXN
line:172: M73I<*'6R;%P_<4UH='2P8#I<+UPO7UY<)ETJ8"DJ+UPQ+3TM+3TO10R(@8`H@
line:173: M("`@("`@("`@("`M93`B<R]<+G6R;%P_<4TO+V<B(%P*("`@("`@("`@("`@
line:174: M+65@(G,O=8)L8#]Q/3\O10R(@8`H@("`@("`@("`@("`M93`B<R]<+BTM+3TM
line:175: M+R]G(B!\(%P*("`@("`@("!P98)L("U-56)).CI%<V-A<&5@+8!E("=P<FEN
line:176: M="!U<FE?=7YE<V-A<&5H)%\I)R!\(%P*("`@("`@("!S961@+65@(G,O+3TM
line:177: M+3TO8%Q<;B]G(BD*"B`@("`C($2E=&6C="!S=8!P;W)T961@<VET91H@("`@
line:178: M10F]R(&-I=&6?;&EN93!I;B`D*&6C:&\@+65@(B2[1TE418TB*4L@10&\*("`@
line:179: M("`@("!I10B!;("(D*&6C:&\@(B2[9VET96]L:7YE?3(@?"!G<F6P("UE(")W
line:180: M97)C87-H93(I(B`]("(B(%T[('2H97X*("`@("`@("`@("`@10F]R('-I=&5@
line:181: M:7X@(B2[5U505$]26$6$5TE416-;1%U](CL@10&\*("`@("`@("`@("`@("`@
line:182: M(",@97-H;R`D9VET96]L:7YE"B`@("`@("`@("`@("`@("!L>8)I9W-?<VET
line:183: M96]55DP](B(*("`@("`@("`@("`@("`@(&QY<FEC<U]R972U<FX](B(*"B`@
line:184: M("`@("`@("`@("`@("!I10B!;("(D*&6C:&\@(B2[9VET96]L:7YE?3(@?"!G
line:185: M<F6P("UE("(D>W-I=&6](BDB("$]("(B(%T[('2H97X*("`@("`@("`@("`@
line:186: M("`@("`@("!L>8)I9W-?<VET96]55DP](B2[9VET96]L:7YE?3(*("`@("`@
line:187: M("`@("`@("`@("`@("!L>8)I9W-?<F6T=8)N/21H10V6T4'ER:7-S("(D>VQY
line:188: M<FEC<U]S:72E8U524'TB*1H@("`@("`@("`@("`@("`@("`@(&QY<FEC<U]R
line:189: M972U<FY?9V]D94TB)"AE9VAO("(D;'ER:7-S8W)E='6R;B(@?"!G<F6P("UO
line:190: M("=#;V2E(%LP+4E=*CHG*3(*"B`@("`@("`@("`@("`@("`@("`@:69@7R`B
line:191: M)&QY<FEC<U]R972U<FY?9V]D93(@(4T@(D-O10&5@-#`R.B(@+7\@(B2L>8)I
line:192: M9W-?<F6T=8)N8V-O10&5B("$](")#;V2E(#1P,SHB(%T[('2H97X*("`@("`@
line:193: M("`@("`@("`@("`@("`@("`@9G)E87L@,@H@("`@("`@("`@("`@("`@("`@
line:194: M(&10I"B`@("`@("`@("`@("`@("`C(&6L<V5*("`@("`@("`@("`@("`@(",@
line:195: M("`@(&6C:&\@(BTM+3TM($-O10&5@-#`Q.B!,>8)I9W,@<VET93!N;W1@10F]U
line:196: M;F1N(@H@("`@("`@("`@("`@("`@10FD*("`@("`@("`@("`@10&]N91H@("`@
line:197: M("`@(&10I"B`@("!D;VYE"@H@("`@97-H;R`B)&QY<FEC<U]R972U<FXB"GT*
line:198: M"B,@37%I;@IW:&EL93`Z"F2O"B`@("!S=&%T94TB)"AO<V%S9W)I<'1@+65@
line:199: M)W2E;&P@88!P;&EC872I;VX@(FE5=7YE<R(@=&\@<&QA>66R('-T872E(&%S
line:200: M('-T<FEN10R<I(@H@("`@;8-G<W2R/3)I6'6N98,@:8,@9W6R<F6N=&QY("2[
line:201: M<W2A=&6]+B(*("`@('=I10'2H/3(D*'2P=71@9V]L<RDB"B`@("!H97EG:'1]
line:202: M(B1H='!U="!L:7YE<RDB"B`@("!L97YG=&@](B2[(VUS10W-T<GTB"@H@("`@
line:203: M:69@7R`B)'MS=&%T98TB(#T@(G!L88EI;F<B(%T[('2H97X*("`@("`@("`C
line:204: M(%2R87-K($%R=&ES=`H@("`@("`@(&%R=&ES=#U@;W-A<V-R:8!T("UE("=T
line:205: M97QL(&%P<&QI9V%T:7]N(")I6'6N98,B('2O(&%R=&ES="!O10B!C=8)R97YT
line:206: M(%2R87-K(&%S('-T<FEN10R=@"@H@("`@("`@(",@6')A9VL@4F%M91H@("`@
line:207: M("`@(%1205-+8TY!345]9&]S88-C<FEP="`M93`G=&6L;"!A<'!L:7-A=&EO
line:208: M;B`B:52U;F6S(B!T;R!N87UE(&]F(&-U<G)E;G1@6')A9VL@88,@<W2R:7YG
line:209: M)V`*("`@("`@("`*("`@("`@("!I10B!;("(D>T],2%]45D%#3WTB("$]("(D
line:210: M>U1205-+8TY!346](B!=.R!T:&6N"B`@("`@("`@("`@(&-L97%R"B`@("`@
line:211: M("`@("`@(&6C:&\@(D-U<G)E;G1Z("2[88)T:8-T?3`M+3T@)'M45D%#3U].
line:212: M05U%?3(*("`@("`@("`@("`@4TQ$8U1205-+/3(D>U1205-+8TY!346](@H*
line:213: M("`@("`@("`@("`@9W6R<F6N=%1205-+4'ER:7-S/3(D*&]S88-C<FEP="`M
line:214: M93`G=&6L;"!A<'!L:7-A=&EO;B`B:52U;F6S(B!T;R!L>8)I9W,@;V9@9W6R
line:215: M<F6N="!5<F%C:R<@?"!T<B`G8'(G("=<;B<I(@H@("`@("`@("`@("!I10B!;
line:216: M("$@+8H@(B2[9W6R<F6N=%1205-+4'ER:7-S?3(@74L@=&AE;@H@("`@("`@
line:217: M("`@("`@("`@97-H;R`B+3TM+3TM+3TM+3!"15=)4CH@4'ER:7-S(&10R;VT@
line:218: M:52U;F6S+B(*("`@("`@("`@("`@("`@(&6C:&\@+65@(B2[9W6R<F6N=%13
line:219: M05-+4'ER:7-S?3(*("`@("`@("`@("`@("`@(&6C:&\@(BTM+3TM+3TM+3T@
line:220: M15Y$.B!,>8)I9W,@10G)O;3!I6'6N98,N(@H@("`@("`@("`@("`@("`@97-H
line:221: M;R`B+3(*("`@("`@("`@("`@97QS91H@("`@("`@("`@("`@("`@97-H;R`B
line:222: M+3TM+3TM+3TM+3!397%R9VAI;F<@;'ER:7-S+BXN(@H@("`@("`@("`@("`@
line:223: M("`@(R!44T2/.B!R97UO=F5@<&QA=&10O<FT@10&6P97YD97YT(&-H88)A9W2E
line:224: M<G,@10G)O;3`D6%)!1TM?4D%-11H@("`@("`@("`@("`@("`@<V6A<F-H4'ER
line:225: M:7-S("(D>U1205-+8TY!346]("2[88)T:8-T)3`J?3(*("`@("`@("`@("`@
line:226: M10FD*("`@("`@("!F:1H@("`@10FD*"B`@("!T<'6T(&-U<"`B)"@H:&6I10VAT
line:227: M("T@,BDI(B`B)"@H=VED=&@@+3!L97YG=&@I*3([('2P=71@97PQ.R!E9VAO
line:228: B("UN("(D>VUS10W-T<GTB"@H@("`@<VQE98`@-PID;VYE"@``
line:229: `
line:230: end
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
