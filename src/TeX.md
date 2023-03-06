# TeX

WYSIWYGではない，マークアップ言語による組版システム．
数学者ドナルド・クヌースが自著の組版の為に作成した．現在はオープンソースコミュニティにより開発されている．
数式組版に強く，理数系の論文作成ではデファクトスタンダードになっている．

オリジナルのTeXは日本語に対応していないが，日本語にも対応した派生版が存在し，広く使われている．

## リンク
- [TeX Users Group](http://www.tug.org/index.html)
  - 一番公式っぽいサイト。TeXLiveなどはここで配布されている。
- [Japanese TeX Development Community（日本語TeX開発コミュニティ ）](https://texjp.org/)
  - 日本のTeX（pTeX等）を現在開発しているところ。
- [TeX Wiki](https://texwiki.texjp.org/)
  - 三重大の奥村先生がつくったWiki。とりあえずこれ。
- [ En toi Pythmeni tes TeXnopoleos［電脳世界の奥底にて］ ](http://zrbabbler.sp.land.to/)
  - ZR氏のサイト。pLaTeX以外の処理系やTeX言語の話など、普通ではない話題についてはここが詳しい。
- [amsmathの数式環境まとめ](https://qiita.com/t_kemmochi/items/a4c390b4967b13f3afb7)
  - 数式を書くための諸々の環境のまとめ
- [texjporg/jfontmaps](https://github.com/texjporg/jfontmaps)
  - pLaTeXでフォント埋め込みを行うときのための標準添付フォントマップの一覧
- [LaTeX Templates](http://www.latextemplates.com/)
  - LeTeXのテンプレートまとめサイト

## 図
### 基本
プリアンプルで

```latex
\usepackage[dvipdfmx]{graphicx}
```

したあと、必要な場所でで

```latex
\begin{figure}
  \centering
  \includegraphics[width=5cm]{apple.png}
  \caption{林檎の図}
\end{figure}
```

### ページに背景を入れる
- [How to use background image in LaTeX?](https://tex.stackexchange.com/questions/167719/how-to-use-background-image-in-latex)

LuaLaTeXを使う場合はbackgroundパッケージを使うのが楽か。これについては[[LuaLaTeX]]を参照。

TeXの層で行わずにTeXの吐いたPDFに後から背景を合成する手もある。qpdfのバージョン8.4以降でこれを行う機能がついたので、qpdfを使えばできるはず。

## 透過PNGにする
### ImageMagickを使う方法
```sh
$ convert -density 600 input.pdf output.png
```

- ポリシーがどうこうといわれて動かない場合は、ポリシー（`/etc/ImageMagick-6/policy.xml`）を書き換えると使えるようになる。
  - 詳細 -> [convertに関して – kondolab](http://zairyo.susi.oita-u.ac.jp/wordpress/?p=7385)
  - セキュリティを低くする設定であるため注意。

## PDFをコンテンツの大きさで切り抜く（cropする）
TeXに他のPDFを貼り付けたい時に余白を切り抜く方法。
TeXLiveについてくる`pdfcrop`というコマンドラインツールを使う。めちゃくちゃ簡単。

```shell
$ pdfcrop hoge.pdf
```

- [pdfcrop - TeX Wiki](https://texwiki.texjp.org/?pdfcrop)

## VSCode + WSL + LaTeX Workshop の設定
- 個人的な環境構築の覚え書き
- プロジェクトごとにpLaTeX、LuaLaTeXを使い分ける想定

### 基本設定（全体の設定）
- Latex › Recipe: Default をlastUsedにする
    - これでプロジェクトごとにLaTeXのエンジンを使い分けるのが簡単になる
    - そのプロジェクトを最初にビルドするときにだけアクティビティバーから Recipe: latexmk(latexmkrc)を選ぶ

### pLaTeXの場合
#### latexmkrc
- プロジェクトのディレクトリに`latexmkrc`という名前で置いておく。`.latexmkrc`という名前で隠しファイルにしてもよい（が、どこかのサイトで非推奨と言われていた）。

```perl
$latex     = 'uplatex %O -synctex=1 -interaction=nonstopmode %S';
$pdflatex  = 'pdflatex %O -synctex=1 -interaction=nonstopmode %S';
$lualatex  = 'lualatex %O -synctex=1 -interaction=nonstopmode %S';
$xelatex   = 'xelatex %O -no-pdf -synctex=1 -shell-escape -interaction=nonstopmode %S';
$biber     = 'biber %O --bblencoding=utf8 -u -U --output_safechars %B';
$bibtex    = 'upbibtex %O %B';
$makeindex = 'upmendex %O -o %D %S';
$dvipdf    = 'dvipdfmx %O -o %D %S';
$dvips     = 'dvips %O -z -f %S | convbkmk -u > %D';
$ps2pdf    = 'ps2pdf %O %S %D';
$pdf_mode  = 3; # ここを3にするとdvi経由でpdfが生成される（$latexの設定が使われる）
```

#### テンプレート
```latex
\documentclass[uplatex,dvipdfmx,ja=standard,a4paper]{bxjsarticle}
\title{題名}
\date{2020-04-16}
\author{著者}
\begin{document}
\maketitle
\section{ほげ}
ふがふが
\end{document}
```
