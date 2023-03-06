# PDF
Portable Document Formatの略。
印刷時の再現性が高い状態で文書を交換できるファイル形式。

## QPDF
PDFを編集したり人間可読化したりできるコマンドラインツール。
[QPDF - TeX Wiki](https://texwiki.texjp.org/?QPDF)を参照。

- [QPDF: A Content-Preserving PDF Transformation System](http://qpdf.sourceforge.net/): 公式サイト

### インストール
公式サイトの指示に従う。

頻繁にアップデートされて新機能が追加されるため、（Linuxディストリビューションのリポジトリのものを使うのではなく）ソースからビルドして最新版を使うとよいかもしれない。

ふつうの*nixな環境では、ビルドは

```sh
$ tar xzf qpdf-9.1.0.tar.gz
$ cd qpdf-9.1.0
$ ./configure
$ make
$ make check #任意
$ sudo make install
```

という標準的な方法でできる。

- ソースからのインストール後、起動しようとしても `qpdf: error while loading shared libraries: libqpdf.so.26: cannot open shared object file: No such file or directory` のようなエラーが出る場合は、`/usr/local/lib`内が共有ライブラリのリンク先になっていないのが原因。
  - 最も簡単な解決策は `$ LD_LIBRARY_PATH=/usr/local/lib qpdf`のように呼び出すこと。aliasにしてもよい。
  - システム全体で`/usr/local/lib`を共有ライブラリの検索先に追加したい場合は、`ldconfig`を使って設定する。

### ドキュメント
オンラインで最新版のマニュアルを読みたいなら[QPDF Manual](http://qpdf.sourceforge.net/files/qpdf-manual.html) を参照（[PDF版](http://qpdf.sourceforge.net/files/qpdf-manual.pdf)もある）。

現在インストールされているバージョンのマニュアルをオフラインで読みたいなら、

- HTML版: `/usr/share/doc/qpdf/qpdf-manual.html`
- PDF版: `/usr/share/doc/qpdf/qpdf-manual.pdf`

を参照（この場所は `man qpdf` に書かれている）。

### PDFのパスワードを解除
```sh
$ qpdf in.pdf --decrypt --password=hoge out.pdf
```

- PDFには閲覧パスワードとマスターパスワードがあり、閲覧パスワードと別のマスターパスワードを設定しておくことで「閲覧はできるが編集はできない」ような権限を他者に与えることができる。が、閲覧さえできてしまえば「それと同じ要素をもつPDFをつくる」という方法でパスワード保護を解除したPDFをつくることができてしまう（qpdfはそれをやってくれる）。

### PDFを結合
複数のPDFファイルを並べたものを1つのPDFファイルとして保存するには、次のようにする。

```sh
$ qpdf in1.pdf --pages in1.pdf in2.pdf in3.pdf -- out.pdf
```

最初に指定した`in1.pdf`は、「メタ情報を`in1.pdf`から引き継ぐ」という意味をもつ。これを引き継ぎたくない場合は、代わりに

```sh
$ qpdf --empty --pages in1.pdf in2.pdf in3.pdf -- out.pdf
```

とすればよい。

### 片面ADFでスキャンしたPDFを結合
Brotherの家庭用プリンターにはADF（自動原稿送り装置）がついているものがあるが、機種によっては両面を同時に読み込むことができない（例えば私の持っているDCP-J957N）。

この片面のADFで両面原稿を（2回に分けて）読み込むと

- 1, 3, 5, 7, ... と奇数ページが正順に並んだPDF
- ..., 8, 6, 4, 2 と偶数ページが逆順に並んだPDF

の2つができる。これを正しい順番で結合したい。

#### 初等的な方法
これをQPDFで行うには、前者を`odd.pdf`、後者を`even.pdf`としたとき、もしそれぞれ2ページなら

```sh
$ qpdf --empty --pages odd.pdf 1 even.pdf 2 odd.pdf 2 even.pdf 1 -- out.pdf
```

とすればよい。これで結合結果が`out.pdf`として吐かれる。

これをページ数に関係なく行うPerlスクリプトは以下の通り。

```perl
#!/usr/bin/env perl
use strict;
use warnings;
use 5.014;
use utf8;
use open ':encoding(UTF-8)';
use Encode::Locale;
binmode(STDIN, ":encoding(console_in)");
binmode(STDOUT, ":encoding(console_out)");
binmode(STDERR, ":encoding(console_out)");
Encode::Locale::decode_argv;

my $odd = "odd.pdf";
my $even = "even.pdf";

# ページ数を取得
my $npages = `qpdf --show-npages $odd`;
chomp $npages;

# 結合
my $args = '';
for my $i (1..$npages){
    my $ri = $npages - $i + 1;
    $args .= "$odd $i ";
    $args .= "$even $ri ";
}
system("qpdf --empty --pages $args -- out.pdf")
```

#### collateを使う方法（バージョン8.3以降）
これはよくあるシチュエーションなのか、バージョン8.3で`--collate`オプションが追加された。これを使うと上記スクリプトと同様の処理が次のように1行で書ける。

```sh
$ qpdf --collate --empty --pages odd.pdf 1-z even.pdf z-1 -- out.pdf
```

- `--collate`を指定すると、与えられた複数のファイルから1ページずつ順にとって結合するようになる。
- `z`は最終ページを表す。
- よって上のように指定すると「oddの1ページ目→evenのzページ目→oddの2ページ目→evenのz-1ページ目→……」という順で結合されるようになる。

### ページに背景を追加する（バージョン8.4以降）
`--underlay`オプションを使うとPDFに背景を追加できる。

「複数ページのPDF文書(main.pdf)のすべてのページに、ロゴなどの入った背景PDF(background.pdf)の1ページ目を背景として追加する」という最も単純な例は次のようになる。

```sh
$ qpdf main.pdf --underlay background.pdf --repeat=1 -- out.pdf
```

### ページを回転させる
全ページを右に90度回転させるには次のようにする。

```sh
$ qpdf in.pdf --rotate=+90 -- out.pdf
```

- 一部のページのみを回転させる場合は角度のあとに`:`をつけ、そのあとにページの範囲を書く。例えば2, 4, 6ページのみ回転させたい場合には、 `--rotate=+90:2,4,6` とする。
