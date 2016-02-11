# バシュっと歌詞のテキストデータを取得できる bash スクリプト

## 概要
これは、いくつかの歌詞表示サイトからテキストな歌詞データを取得する bash スクリプトです。スクリプトは PNG に埋め込みました。(なんだかコンテナみたい。)埋め込んだスクリプトはワンライナで取り出すことができます。暇だったら挑戦してみてください。ド定番のコードからこのスクリプトでしか使わないような正規表現が入っているので、シェルおじさんになりたいマンやシェルに興味ありウーマンの方々が挑戦してくれると嬉しいです。

なお、現状洋楽には弱いです。壊滅的です。今後対応します。対応サイトも増やす予定です。

!["ver 2 スクショ"](./ScreenShot.png)

埋め込んであるスクリプトは GNU 系を回避したはずなので、UNIX / Unix like で動くと思います。当方 EL Capitan で動作確認はしています。

## 仕様
- ver1
    1. Ready for UNIX / Unix like
    2. URL を引数にして実行します。(URL はヒトが探します。)
    3. awk, sed, curl を駆使し、テキストデータだけ抜き出します。
- ver2
    1. Ready for UNIX / Unix like
    2. curl で Google 検索し、awk, sed を駆使して検索結果から歌詞ページの候補 URL を抜き出します。
    3. 候補 URL から現状対応しているサイトを探し curl でアクセスします。
    4. awk, sed, curl を駆使し、テキストデータだけ抜き出します。
- ver3
    1. Ready for only Mac
    2. osascript コマンドで iTunes で再生中の曲タイトルを取得します。
    3. 歌詞を探して標準出力します。
    4. while で無限ループしてますが、フラグ入れているので無限に探すことはしません。
    5. 歌詞がすでに iTunes 内にあればそれを標準出力します。
    6. 歌詞がなければネットから拾ってきて標準出力しつつ iTunes に保存します。(iCloud 上の楽曲についてはどうなるか分かりません)

各サイトともコピペされないよう色々対策していて面白いです。

## 問 1
下記画像から該当スクリプトを取り出しなさい。

!["Q1"](./aaa.png)

version 1, このバージョンでは、引数に歌詞ページの URL を与えると歌詞データを取得します。
```
% this_script "URL"
```

## 問 2
下記画像から該当スクリプトを取り出しなさい。

!["Q2"](./bbb.png)

version 2, このバージョンでは、引数に曲タイトルやタイトルの一部を与えると歌詞データを取得します。上記スクショをご参照ください。
```
$ this_script "楽曲タイトル"
or
$ this_script "楽曲タイトルの一部 キーワード など 空白で区切る"
```
ex)
```
$ ./this_script "ヒツジ すべてがFに ed"
: ...snip...
----- BEGIN lyric
割り切れない感情
もどかしいほどに
:
:
```

## 問 3
とりあえず動いているんですが、バグ探し中。

version 3, このバージョンでは、引数は必要ありません。
```
$ open -A iTunes
$ this_script
```

## 問 4
画像ではない何かの予定

## 答え
そのうち載せます。

## License
Copyright (c) 2016 tkumata

This software is release under the MIT License, please see [MIT](http://opensource.org/licenses/mit-license.php)

## Author
[tkumata](https://github.com/tkumata)
