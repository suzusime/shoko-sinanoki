# IPython
PythonのREPLのつよいやつ。

## リンク
- [Jupyter and the future of IPython — IPython](https://ipython.org/index.html): 公式サイト
- [IPython Documentation](https://ipython.readthedocs.io/en/stable/): 公式ドキュメント

## 魔法命令
IPython上では`%`または`%%`から始まる特殊な命令を使うことができる。これを魔法命令(magic commands)または魔法函数(magic functions)という[^1]。

[^1]: 日本語圏のサイトで訳しているのを見たことはないが、中国語圏ではこう訳しているようである。

- [Built-in magic commands](https://ipython.readthedocs.io/en/stable/interactive/magics.html): 魔法命令の一覧

```python
# 魔法命令の一覧を表示
%lsmagic

# エディタを表示してその保存結果を実行
%edit

# スクリプトを読み込み（次のInに入力される）
%load myscript.py

# スクリプトの実行
%run myscript.py

# セル番号3の内容を取得
%recall 3
```

## Qt Console
Jupyter Notebookを使うほどではないがグラフはコンソールに表示してほしい、というときに使える高機能端末。

- [非エンジニアでも使いやすい高機能なPython環境「IPython」「Jupyter」を使ってみよう](https://knowledge.sakura.ad.jp/17727/)
- [ipython notebookでmatplotlibでプロットした図が小さい](https://github.com/eisoku9618/kuroiwa_demos/issues/69)

```sh
$ jupyter qtconsole # 起動
```

```python
# Qt Console上の入力例
# 出典: https://knowledge.sakura.ad.jp/17727/
%matplotlib inline
import matplotlib
matplotlib.pyplot.rcParams['figure.figsize'] = (15.0, 15.0) # グラフを大きくする
matplotlib.pyplot.plot([x*x - 10*x + 15 for x in range(11)]) # 2次函数のグラフを描画
```
