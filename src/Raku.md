# Raku(Perl6)
もともとPerl6と呼ばれていたスクリプト言語。
2000年に開発開始され、2015年に正式版がリリースされた。Perl5とは互換性がないため、2019年にRakuと改名された。名前の由来は日本語の「楽」である。

言語自体が非常に多機能。特徴的なのは

- 強力な文字列マッチング: PEG(Parsing expression grammar)と同じ能力をもつ
    - [Raku rules - Wikipedia](https://en.wikipedia.org/wiki/Raku_rules)
    - [Perl 6の正規表現について - Qiita](https://qiita.com/sinya8282/items/361f13409a0ca9e3e66c)
- Unicodeへの充実したサポート: ソースコードに文字としてUnicode文字が使えるのはもちろん、正規化や不正なUTF-8バイト列の処理などもできる
- promiseやasync/awaitなどを用いた並列実行
- Cのライブラリを呼び出すのが比較的簡単
- コマンドラインオプションの自動生成

あたりか。

- [Raku Programming Language](https://raku.org/): 公式サイト
    - [Raku Documentation](https://docs.raku.org/): ドキュメント
    - [Raku Modules Directory](https://modules.raku.org/): モジュール（ライブラリ）検索
    - [FAQ](https://docs.raku.org/language/faq): よくある質問

## リンク集
- [Raku 入門](https://raku.guide/ja/)
- [My books – Andrew Shitov](https://andrewshitov.com/): PDFが無料配布されている
    - [Perl 6 at a Glance](https://andrewshitov.com/perl6-at-a-glance/)
    - [Using Raku](https://andrewshitov.com/2019/10/13/using-raku-the-free-book/)
    - [Raku One-Liners](https://andrewshitov.com/2019/10/18/raku-one-liners-a-free-book/)
    - ref. [Raku言語(Perl 6)のクックブック「Using Raku」が無料で公開。他にも学習本を紹介 - 実録コンピュータ物語](http://cabonera.hateblo.jp/entry/2019/10/14/123305): 各書の立ち位置の解説

## インストール
[rakubrew - Raku installer and version manager](https://rakubrew.org/)（rbenv等のenv系のRaku版相当）を使うと様々な環境で同様にインストールできる（多くのバージョンではバイナリがダウンロード可能）。

Rakubrewのインストール自体は、ファイルを直接パスの通った場所に置くほか、Perlを使っているならCPANより

```
$ cpanm App::Rakubrew
```

でインストール可能。その後は

```bash
$ rakubrew init # 最初に初期化処理が必要。指示に従った操作をする
$ rakubrew available # 入手可能なバージョンを表示。"D"付きのバージョンはバイナリが入手可能
$ rakubrew download 2021.02.1 # バージョンを指定してダウンロード・インストール
```

のようにすれば良い。他もrbenvなどと同様に使える。詳細は公式サイトまたは`man rakubrew`を参照。

## Rakudo
- [Rakudo Compiler - Rakudo Compiler for Raku Programming Language](https://rakudo.org/)
- [rakudo/docs at master · rakudo/rakudo](https://github.com/rakudo/rakudo/tree/master/docs): ドキュメント
    - [running.pod](https://github.com/rakudo/rakudo/blob/master/docs/running.pod): `raku`コマンドの使い方
    - [compiler_overview.pod](https://github.com/rakudo/rakudo/blob/master/docs/compiler_overview.pod): rakudoコンパイラの動作概要

Raku自体は言語仕様（正確にはテストケースの集合体）であって、この仕様に準ずる（テストに通る）ものがRakuの処理系として認められる（このあたりは「処理系即ち仕様」なPerlとは異なる部分）。
Rakudo（楽土）は2020年9月現在の最も有力な処理系である。

## Zef
- [ugexe/zef: Raku / Perl6 Module Management](https://github.com/ugexe/zef)

Rakuのパッケージマネージャ。

以下、配布ページにある使い方の抄訳。

```sh
zef --help
zef --version

# 頒布物のインストール
zef install CSV::Parser

# 名前による頒布物検索
zef search CSV

# 頒布物の詳細表示
zef info CSV::Parser

# 全ての入手可能な頒布物の列挙
zef list

# ある頒布物に依存している頒布物の列挙
zef rdepends HTTP::UserAgent

# 現在のディレクトリのプロジェクトをテスト
zef test .

# 頒布物の取得（インストールはしない）
zef fetch CSV::Parser

# 頒布物を取得してそこに移動
zef look CSV::Parser

# smoke test modules from all repositories（これ何？）
zef smoke

# 指定されたパスにある Build.pm の実行
zef build .

# レポジトリの頒布物リストを更新（ベータ）
zef update

# 全ての頒布物の更新（ベータ）
zef upgrade

# 特定の頒布物の更新（ベータ）
zef upgrade CSV::Parser
```

なお、githubからのインストール直後は非常に古いリポジトリを参照していた（2020年9月現在）。
インストール後は`zef update`を推奨。

## rakudoc
- [Raku/rakudoc: A tool for reading Raku documentation](https://github.com/raku/rakudoc)

コマンドラインでドキュメントを読むツール。perldocのRaku版。以前の名前はp6doc。

- 2020年9月現在、ドキュメントに沿ってインストールしようとすると失敗するので [cannot install · Issue #5 · Raku/rakudoc](https://github.com/Raku/rakudoc/issues/5) を参照して、`zef install .`を叩く。私は`dot:from<bin>`が足りないと言われて困っていたのだが、これが`dot`という名前のコマンドラインツールがないという意味だったとは……（`dot`を提供するgraphvizをインストールしたら解決した）。

## ファイル入出力
- [Input/Output](https://docs.raku.org/language/io)
- [Input/Output the definitive guide](https://docs.raku.org/language/io-guide)

```perl
my $txt = "DASS ich dereinst, an dem Ausgang der grimmigen Einsicht,
Jubel und Ruhm aufsinge zustimmenden Engeln.
Daß von den klargeschlagenen Hämmern des Herzens
keiner versage an weichen, zweifelnden oder
reißenden Saiten. Daß mich mein strömendes Antlitz
glänzender mache: daß das unscheinbare Weinen
blühe. O wie werdet ihr dann, Nächte, mir lieb sein,
gehärmte."; # 適当なサンプルテキスト

# ファイルへの書き込み
"test.txt".IO.spurt: $txt;

# ファイルからの読み込み
my $poem = "test.txt".IO.slurp;
say $poem;

# 以下のような伝統的な(?)方法もある
# ファイルへの書き込み
my $fh1 = open "test2.txt", :w;
$fh1.print($txt); # なお、printの代わりにsayを使うと最後に改行が入る
$fh1.close;

# ファイルからの読み込み
my $fh2 = open "test2.txt", :r;
my $contents = $fh2.slurp;
$fh2.close;
say $contents;
```

## テキスト操作
- [class Str](https://docs.raku.org/type/Str): 文字列クラス
- [Regexes](https://docs.raku.org/language/regexes): 正規表現解説。Rakuの正規表現は独特なので注意。

```perl
my $txt = "DASS ich dereinst, an dem Ausgang der grimmigen Einsicht,
Jubel und Ruhm aufsinge zustimmenden Engeln.
Daß von den klargeschlagenen Hämmern des Herzens
keiner versage an weichen, zweifelnden oder
reißenden Saiten. Daß mich mein strömendes Antlitz
glänzender mache: daß das unscheinbare Weinen
blühe. O wie werdet ihr dann, Nächte, mir lieb sein,
gehärmte.";

say $txt; # 表示
print $txt; # 改行なしで表示
dd $txt; # デバッグ用出力

# 以下出力関数は省く
$txt.uc; # 全部大文字に
uc $txt; # だいたいの函数は前置と後置両方できる

$txt.lines; # 各行を要素とした配列を返す
$txt.lines.grep: *.contains: "der"; # 配列から "der" を含む要素のみ選ぶ
.say for $txt.lines.grep: *.contains: "der"; # "der"を含む行のみ出力

$txt.lines.grep: *.match: /\,$/; # カンマで終わる行のみ取り出す
$txt.words.grep: *.match: / ^ \w ** 4..6 $ /; # 4文字以上6文字以下の単語のみ取り出す

$txt.subst(/der/, "でる"); # 最初の/der/にマッチする文字列を"でる"に置換して返す
$txt.subst(/der/, "でる"):g; # 全ての/der/にマッチする文字列を"でる"に置換して返す

# Unicode関係
my $kana = "ご注文はココアですか？";
$kana.uninames.map: *.say; # 各文字のUnicode名を表示
($kana.uninames.map: *.subst("KATAKANA", "HIRAGANA").uniparse).join; # 片仮名を平仮名に変換
```

- sayとprintには改行の有無以外にも違いがある。[FAQ](https://docs.raku.org/language/faq#How_and_why_do_say,_put_and_print_differ?)を参照。

## Font::FreeType
- [Font-FreeType-raku | Read font files and render glyphs using FreeType2](https://pdf-raku.github.io/Font-FreeType-raku/)

Freetype2のよくできたラッパーがある。RakuはUnicodeサポートがしっかりしているのもあって使いやすそう。

```sh
# 注意：インストール前にOSにfreetype2のライブラリを入れておく
$ zef install Font::FreeType # インストール
```

使い方：

```perl
use Font::FreeType;
use Font::FreeType::Face;

my Font::FreeType $freetype .= new;
my Font::FreeType::Face $face = $freetype.face('kingen.otf');

$face.set-char-size(24, 24, 100, 100);

# 各グリフに対してコールバックを呼べる
$face.for-glyphs: '鐘の音', -> Font::FreeType::Glyph $g {
    say "glyph {$g.Str()} has size {$g.width} X {$g.height}"; # グリフのサイズを表示
    my $g-image = $g.glyph-image;
    ($g.Str() ~ '.pgm').IO.open(:w, :bin).write: $g-image.bitmap.pgm; # グリフをpgm画像ファイルに保存
    # 要するに "鐘.pgm", "の.pgm","音.pgm" の3ファイルが生成される
}
```
