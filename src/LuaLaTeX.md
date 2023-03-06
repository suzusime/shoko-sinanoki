# LuaLaTeX
Luaが使えるTeX処理系LuaTeXにLaTeXを組み込んだもの。実行は遅いが、かなり高度な組版ができる。
DVIを経由せずPDFを直接出力するため、フォントの情報を用いた組版ができたり、画像周りの難点が下がったりしている。

同様にPDFを直接出力するTeXとしてpdf(La)TeXがあり海外ではこれが標準の地位を占めつつあるが、pdfLaTeXではあまりきちんとした日本語組版ができないため、pdfLaTeXの系譜であるLuaLaTeXが日本では「未来の標準TeX」として期待されているといった状況（2019年11月現在）。
「pdfLaTeXの後継がLuaLaTeXであること」は決定しているらしい[^1]。
前述の事情から海外で最近作られたパッケージの中にはpdfLaTeX向けでありpLaTeXでは動かないものがあるが、そういうものがLuaLaTeXでは動いてくれるという嬉しさもある。

[^1]: <http://zrbabbler.sp.land.to/lualatexlua.html>

なお、LuaTexの側はLaTeXではなくConTeXtを念頭に開発しているので、LaTeXとの関係はあまり考慮されていないんだとか……[^2]。

[^2]: <http://acetaminophen.hatenablog.com/entry/2018/12/09/171231>

## 実行
1コマンドでtexソースからpdfができあがる．

```sh
$ lualatex hoge.tex
```

## Hello, world
bxjsclsを使うと概ねpLaTeXと同じような使い勝手で使えるのでおすすめ．
このドキュメントクラスは，（以下のように設定すると）内部で日本語文書のためのパッケージ（LuaTeX-jaと`luatexja-fontspec`）を読み込んでくれる．

```latex
\documentclass[lualatex, ja=standard]{bxjsarticle}
\begin{document}
こんにちは世界！
\end{document}
```

他のドキュメントクラス（とくに英語圏のもの）を使う場合は，フォント設定を適切に行わないと日本語は出力できない．
フォント設定だけでも日本語の文字の出力は可能だが，和欧混植のような日本語の文章で普通な組版のために，LuaTeX-jaを使うのがよいと思う．

- [LuaTeX-ja - TeX Wiki](https://texwiki.texjp.org/?LuaTeX-ja)

## フォント設定
以下ではluatexja-fontspecを前提とした説明を行う。

詳細は

```sh
$ texdoc luatexja # LuaTeXja及びluatexja-fontspecに固有の情報
$ texdoc fontspec # fontspec一般の情報
```

を参照。
luatexja-fontspecは、fontspecという一般的なフォント選択用パッケージに対して日本語組版のための拡張（和文書体・欧文書体を別に扱う等）を施したものであるため、fontspecの基本的な機能についての解説はluatexja側のドキュメントには書かれておらず、fontspec側のドキュメントを参照する必要がある。

### デフォルト和文フォントの指定
```latex
\documentclass[lualatex, ja=standard]{bxjsarticle}

% メイン和文フォント（～明朝体フォント）の指定
\setmainjfont{J-IwaOMinPr6N-Th}[
  BoldFont={J-IwaOMinPr6N-Bd}
]

% ゴシック体和文フォントの指定
\setsansjfont{J-IwaOGoPr6N-Lt}[
  BoldFont={J-IwaOGoPr6N-Bd}
]

\begin{document}
\section{自由な1粒子の場合のSchrödinger方程式}
自由な1粒子の場合、Schrödinger方程式は次のように書ける。
\end{document}
```

- 明朝体は`\setmainjfont`で，ゴシック体は`\setsansjfont`で行う．
- 何か追加でオプションが必要な場合は，これらのコマンドにオプションとして渡す．
  - 上の例では`BoldFont`（ボールド時に使われるフォント）を明示的に指定している．ただし，`BoldFont`については多くの場合は指定しなくともいいかんじになる．

### beamerの場合の例
以下はbeamerで源ノ角ゴシック + Source Sans Proを使う場合の例。

```latex
\documentclass{beamer}

% luatexja及びluatexja-fontspecを読み込む
\usepackage{luatexja}
\usepackage{luatexja-fontspec}

% フォントを源ノ明朝、源ノ角ゴシックにする(sourcehan)
% \textbf等で和文書体も切り替えるようにする(match)
% 和文サイズ/欧文サイズ = 0.924865 にする(scale)
\usepackage[sourcehan,match,scale=0.924865]{luatexja-preset}

% 和文のデフォルト書体をゴシック体に変更
\renewcommand{\kanjifamilydefault}{\gtdefault}

% 欧文サンセリフ体を Source Sans Pro にする
% 但し太字は Semi Bold にする
\setsansfont[BoldFont={Source Sans Pro Semi Bold}]{Source Sans Pro}

% 和文ゴシック体を源ノ角ゴシックにする
% 但し標準、太字をそれぞれRegular, Boldにする
\setsansjfont[BoldFont={Source Han Sans Bold}]{Source Han Sans Regular}

\title{サンプルSamplingの性質}
\subtitle{すべての人間に向けて}
\author{匿名276}
\date{\today}

\begin{document}
\frame{\titlepage}
\section{概要}
\begin{frame}
    \frametitle{第1の啓示}
    \begin{enumerate}
        \item ほげhoge
        \item \textbf{ふがfuga}
        \item ぴよpiyo
    \end{enumerate}
\end{frame}

\end{document}
```

- 和文書体と欧文書体のフォントサイズは、ふつう1:1にせず和文書体のほうを縮めて見た目がよくなるよう調整する。日本語LuaLaTeX用に作られたドキュメントクラスではこれがあらかじめ設定されていることもあるが、`beamer` にはそのような設定がないので自分で設定する。
  - 上の例の0.924865倍という値は「ltjsarticle クラス使用時には和文は欧文の約 0.924865 倍となる」という記述から取っている。
  - jsclassesでこのような値になっているのは「欧文の10ptを和文の13級にあわせる」という考えに由来する（[出典](https://oku.edu.mie-u.ac.jp/tex/mod/forum/discuss.php?d=1001)）。
    - TeXのポイントは 1 pt = 1/72.27 inch であることから、10 pt≒3.5145980 mm。13級=3.25 mm であるから、これを合わせるためには 3.25 / 3.5145980 = 0.924714 倍を 和文サイズにかけてやればよい。上の 0.924865 とは少し値が異なるが、おそらく歴史的経緯の問題だと思う。
  - ltjarticleの場合は 0.962216 倍なので少し和文が大きい。これはpTeXのJISフォントメトリックの値に由来する（？）。
- beamerではデフォルト欧文書体がサンセリフ体に設定されている。和文もそれに合わせてゴシック体をデフォルト書体にするためにはその旨設定する必要がある。
- fontspecのデフォルトの太さ選択に任せると標準の文字／太字での文字の太さが合わない場合がある（源ノ角ゴシック + Source Sans Proの場合はそう）ので、`BoldFont`オプションを使いつつ調整する。

## 図
### ページに背景を入れる
- [How to use background image in LaTeX?](https://tex.stackexchange.com/questions/167719/how-to-use-background-image-in-latex)

いくつか方法があるが、LuaLaTeXを使うならbackgroundパッケージを使うのが楽そう。

```
\documentclass[lualatex,ja=standard]{bxjsarticle}

\usepackage{graphicx}
\usepackage{background}
\backgroundsetup{
scale=1,
color=black,
opacity=1,
angle=0,
contents={\includegraphics[width=\paperwidth,height=\paperheight]{sample.pdf}}
}

\begin{document}
ほげほげ
\end{document}
```

- [天地有情 \[LaTeX\] background --- 文書のページ上の背景素材の配置](http://konoyonohana.blog.fc2.com/blog-entry-343.html)
- `\includegraphics` したいときは `\usepackage{background}` のオプションではなく、`\backgroundsetup` を使わないといけないことに注意（ドキュメントに書かれている）。

## Lua周り
- [Programming in Lua (first edition) ](https://www.lua.org/pil/contents.html)
  - Luaの公式解説書。最新版は第4版（Lua5.3対応）だが、初版（Lua5.0対応）はインターネットで無料公開されている。
  - 第2版は日本語訳が出版されている（アスキー・メディアワークス、2009年。[Amazonのリンク](https://www.amazon.co.jp/Programming-Lua-%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%9F%E3%83%B3%E3%82%B0%E8%A8%80%E8%AA%9ELua%E5%85%AC%E5%BC%8F%E8%A7%A3%E8%AA%AC%E6%9B%B8-Roberto-Ierusalimschy/dp/4048677977)）。
- [Luaプログラミング入門](https://densan-labs.net/tech/lua/)
  - Lua5.1の解説。読みやすい。
- [思わず Lua で LaTeX してみた ～LuaTeX で日本語しない件について～](http://zrbabbler.sp.land.to/lualatexlua.html)

### Luaでループさせる
TeXコードでループさせるのはいろいろと大変だが、以下のようにLuaのループ構文と`tex.sprint`を使うと比較的簡単に繰り返しが書ける。

細かい挙動（特に特殊文字回り）は正直よくわからないが、`\directlua{\detokenize{ }}`で囲んだ中にLuaのコードを書けばいい感じに動いてくれそうな雰囲気。

```latex
% いろいろなNに対する実験結果のグラフを張り込む例
\directlua{\detokenize{
  for i,value in ipairs({7, 9, 11, 13, 14, 15, 16, 17, 18, 19, 20}) do
    tex.sprint("\\subsection{N=", value, "のとき}")
    tex.print("\\subsubsection{結果}")
    tex.sprint("\\includegraphics[height=9cm]{image/img", value, ".png}")
  end
}}
```
