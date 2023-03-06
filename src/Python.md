# Python
オブジェクト指向なスクリプト言語．Perlの1年後に生まれた．
インデントを構文解析に用いること（オフサイドルール）に特徴がある．
Perlと対照をなす規範として “There should be one-- and preferably only one --obvious way to do it.”（「何かを為すにはひとつの――できればたったひとつの――明快な方法があるべきだ」）を掲げている．
少し前は「海外でよく使われているスクリプト言語」的な紹介のされ方だった気がするが，近年では数値計算やデータ科学，機械学習向けのライブラリが多数Python向けに書かれたため，そちら方面の言語の印象が強い．

Python2系とPython3系との非互換性とそれにより生ずる問題も有名．
LinuxではOSの様々なスクリプトがpythonで書かれているために簡単にPython3に置き換えるわけにはいかないようである．
例えばUbuntuでは，後方互換性のため（Python3が標準インストールされるようになった今でも） `python` で呼ばれるのはPython2，`python3` で呼ばれるのがPython3である．
このページでは，Ubuntu&Python3を前提として`python3` や `pip3`を用いることにする．

## インストール
普通はOSのリポジトリから入れる．
以前はバージョンを共存させられるpyenvでインストールする方法がよく紹介されていたが，最近では「Python標準のvenvで大抵は事足りるよ」という雰囲気．

### Conda
- [Conda — conda 4.8.4.post65+1a0ab046 documentation](https://docs.conda.io/projects/conda/en/latest/)
- [Miniconda — Conda documentation](https://docs.conda.io/en/latest/miniconda.html)

主に数値計算、機械学習方面で使われているパッケージシステムあるいは配布ディストリビューション。
pipではなくcondaというコマンドを使ってパッケージを管理する。condaとpipを両用すると簡単に環境がぶっ壊れると有名なので注意。
全部入りのAnacondaと最小構成のMinicondaがある。Minicondaでも`conda install`で後から必要なパッケージを足せるので、コマンドラインでの操作に抵抗がなければMinicondaがおすすめ。

[[Python/SciPy]]のページでも触れているが、C, C++, FORTRANなどを用いるライブラリはビルド済みのものが配布されるおかげでWindowsでも環境構築でつまずきにくい。
また、このビルド済みパッケージが Intel MKL のようなプロプライエタリなライブラリを用いてビルドされている場合もあり、計算の高速化も見込める。

- [Anaconda の NumPy が高速みたいなので試してみた - Morikatron Engineer Blog](https://tech.morikatron.ai/entry/2020/03/27/100000)

#### bash以外の場合
bashでは Anaconda/Miniconda をインストールすると自動的にcondaにパスが通されるが、zshなどを使っているとこれをしてくれない。
`conda init $SHELL`をすると設定を設定ファイルに書き込んでくれる。

- 参考: [MacOS版Anacondaのインストール - python.jp](https://www.python.jp/install/anaconda/macos/install.html)

## ライブラリのインストール
ライブラリのインストールはpipというコマンドで行う．

たとえば

```sh
$ pip3 scipy seaborn jupyter
```

とでもするとデータ科学系で人気のライブラリがだいたい入る．

- Ubuntuでは管理者権限不要．`~/.local/lib/python3.6/site-packages/`のような場所（ユーザーディレクトリ以下）にインストールされる．
  - 他のディストリビューションでも `--user` オプションをつけると同様の振る舞いをする．
  - Ubuntuでは非特権ユーザーがpipを叩いた場合は自動で `--user` が指定されたものとして動作するような変更が加えられている（出典：[pip - python.jp](https://www.python.jp/install/ubuntu/pip.html)）．
- システムにインストールしたい場合は，Ubuntuならsudoを使い，他のディストリビューションなら`--user`をつけずに呼び出せば良い．
  - ただ，システムにインストールする場合は，OS側のパッケージマネージャでライブラリを入れた方が安心かもしれない．

## ファイル入出力
```python
# ファイルを読み込んで1行ずつ処理
with open('in.txt') as f:
    for line in f:
        #lineは末尾の改行を含むので改行なしで出力
        print(line, end='')

# ファイルを読み込んで全体を文字列に格納
with open('in.txt') as f:
    str = f.read()
    print(str, end='')

# ファイルを読み込んで行ごとに分けてリストに格納
with open('in.txt') as f:
    lines = f.readlines()
    print(lines)

# ファイルに文字列を書き込み
# w のかわりに a にすると追記モード
# w のかわりに x にすると上書き禁止（ファイルが存在しないときのみ書き込む）
with open('out.txt', 'w') as f:
    f.write("hoge\n") # 改行は自動では入らない
    f.write("ふが\n")
```

### JSON
`import json` する（標準ライブラリ）．

- サンプルファイルは[JSON:API — Examples](https://jsonapi.org/examples/)より

```python
import json

# Pythonのヒアドキュメントは二重引用符3つ
src = """
{
  "data": [{
    "type": "articles",
    "id": "1",
    "attributes": {
      "title": "JSON:API paints my bikeshed!",
      "body": "The shortest article. Ever."
    },
    "relationships": {
      "author": {
        "data": {"id": "42", "type": "people"}
      }
    }
  }],
  "included": [
    {
      "type": "people",
      "id": "42",
      "attributes": {
        "name": "John"
      }
    }
  ]
}
"""

# Pythonのオブジェクトにする
decoded = json.loads(src)
print(decoded)

# JSON形式でテキストにする
txt = json.dumps(decoded, sort_keys=True, indent=4)
print(txt)
```

## SciPy, NumPy
[[Python/SciPy]] を参照．

## IPython
[[Python/IPython]] を参照．

## リンク
- [Pythonの変数スコープの話](https://qiita.com/msssgur/items/12992fc816e6adf32cff)
  - Pythonは函数内でもグローバル変数を参照できるが，そのまま代入はできないという話．
  - グローバル変数に代入したいときは `global` キーワードを使う．

## デコレータ
- 何かの函数でくるまれた函数を簡単に定義できる。
- いいかんじの訳語はないのか？

```python
In [1]: def callby2(func):
   ...:     return func(2)
   ...:

In [2]: def squere(n):
   ...:     return n*n
   ...:

In [3]: squere(4)
Out[3]: 16

In [6]: callby2(squere)
Out[6]: 4

In [7]: @callby2
   ...: def cube(n):
   ...:     return n*n*n
   ...:

In [9]: cube
Out[9]: 8
```

## ドキュメンテーション文字列
コメントは`#`だが、ドキュメントとなるような文字列（ドキュメンテーション文字列）は`"""文字列"""`のように書く習慣がある。

```python
def func(a):
    """
    2倍して返す函数
    """
    return a*2
```

- [Pythonのdocstring（ドキュメンテーション文字列）の書き方 | note.nkmk.me](https://note.nkmk.me/python-docstring/)

doctestといって、これをもとにテストを行う方法がある。

- [doctest --- 対話的な実行例をテストする — Python 3.8.0 ドキュメント](https://docs.python.org/ja/3/library/doctest.html)

```python
# dt.py
def func(a):
    """
    2倍して返す
    >>> [func(a) for a in [1, 2, 3, 4]]
    [2, 4, 6, 8]
    """
    return a*2
```

```sh
$ python3 -m doctest dt.py -v
Trying:
    [func(a) for a in [1, 2, 3, 4]]
Expecting:
    [2, 4, 6, 8]
ok
1 items had no tests:
    dt
1 items passed all tests:
   1 tests in dt.func
1 tests in 2 items.
1 passed and 0 failed.
Test passed.
```

## コマンドライン引数
コマンドライン引数の解析を行いたいときは標準ライブラリの argparse モジュールを用いると楽。

- [argparse --- コマンドラインオプション、引数、サブコマンドのパーサー](https://docs.python.org/ja/3/library/argparse.html)

```python
# 例: 2項演算する電卓っぽいプログラム

import argparse

# パーサのインスタンスを作成
parser = argparse.ArgumentParser()

# 順序によって定まる必須引数を追加
# typeは指定しないと文字列になる
parser.add_argument("lhs", type=int, help="第一引数")
parser.add_argument("rhs", type=int, help="第二引数")

# オプションを追加
# デフォルト値を指定できる
parser.add_argument("-m", "--calc-method", help="計算方法", default='add')

# オプションの有無を真偽値で取得したい場合は action に 'store_true' を指定する
parser.add_argument('--reverse', action='store_true', help="引数を逆順にする")

# args にコマンドライン引数の内容を格納する
args = parser.parse_args()

# argsのインスタンス変数のようにして参照できる
lhs = args.lhs
rhs = args.rhs

if args.reverse:
    (lhs, rhs) = (rhs, lhs)

# 引数名・オプション名にハイフンが入る場合はアンダーバーに変換される
method = args.calc_method

if method == 'add':
    print(lhs+rhs)
elif method == 'sub':
    print(lhs-rhs)
elif method == 'mult':
    print(lhs*rhs)
elif method == 'div':
    print(lhs/rhs)
else:
    print("未知の演算が指定されました")
```

以上のように書くと、次のようにヘルプを生成してくれる：

```
$ python3 ap.py -h
usage: ap.py [-h] [-m CALC_METHOD] [--reverse] lhs rhs

positional arguments:
  lhs                   第一引数
  rhs                   第二引数

optional arguments:
  -h, --help            show this help message and exit
  -m CALC_METHOD, --calc-method CALC_METHOD
                        計算方法
  --reverse             引数を逆順にする
```

## 半無限ループ
C++の`for(int i=0; ; ++i){ if(hoge) break; }` みたいなことをするには、イテレータ機能を使う。
この例に相当するものは itertools.count() として標準ライブラリにある（もう少し複雑なものを作るにはクラスを書く必要がある）。

- [itertools --- 効率的なループ実行のためのイテレータ生成関数](https://docs.python.org/ja/3/library/itertools.html)

```python
import itertools
for i in itertools.count():
    print(i)
    if i >= 100:
        break
```

## 函数のメモ化
- [functools --- 高階関数と呼び出し可能オブジェクトの操作 — Python 3.8.6rc1 ドキュメント](https://docs.python.org/ja/3/library/functools.html)

「同じ引数であれば必ず同じ値を返し、かつ副作用を起こさない」（＝参照透過性のある）函数は、`functools.lru_cache()`というデコレータを着けるだけで自動的にメモ化できる。

```python
import functools

@functools.lru_cache(maxsize=None)
def fibo(n):
    if n==0:
        return 0
    elif n==1:
        return 1
    else:
        return fibo(n-2)+fibo(n-1)
```

## venv
仮想環境をつくるやつ。
Pythonの処理系自体は共有し、ライブラリのみを独立して管理することができる（処理系自体を独立して管理したい場合はpyenvなどを使えばできる）。
Python3.3以降で標準添付。下記のコマンドはPython3.5以降のもの。

- [venv --- 仮想環境の作成 — Python 3.8.4 ドキュメント](https://docs.python.org/ja/3/library/venv.html): 公式
- [venv: Python 仮想環境管理 - Qiita](https://qiita.com/fiftystorm36/items/b2fd47cf32c7694adc2e)

### 環境をつくる
```sh
$ python3 -m venv [環境名]
```

カレントディレクトリ以下`[環境名]`にディレクトリが作られ、そこに各種ファイルが入ることになる。
プロジェクトのディレクトリに作ってしまうのが楽？（gitignoreなどを忘れないように）

### 環境の有効化
```sh
$ source [環境の場所]/bin/activate
```
これはLinuxでbashまたはzshを使う場合。
それ以外の場合は公式ドキュメントを参照のこと。

### 環境の無効化
```sh
$ deactivate
```

## モジュールとパッケージ
- [6. モジュール — Python 3.8.4 ドキュメント](https://docs.python.org/ja/3/tutorial/modules.html)

### モジュール
例えばfibo.pyの機能をmain.pyから使いたければ、同じディレクトリに置いて次のようにすればよい。

fibo.py（使われる側）:

```python
def fib(n):    # フィボナッチ数列をnまで出力
    a, b = 0, 1
    while a < n:
        print(a, end=' ')
        a, b = b, a+b
    print()
```

main.py（使う側）:

```python
import fibo
fobo.fib(10)
```

Pythonによるライブラリ参照機能（モジュール）は極めて素朴。importされた側のスクリプトは普通に上から順に実行される。
函数やクラスの定義だけでなく何かしらの処理を書いていた場合はそれも実行されるので注意。

```python
if __name__ == '__main__':
    print("python hoge.py として呼ばれた")
```
のように書くことで、importされたときは呼ばれず、`python hoge.py`のように直接呼ばれたときのみ実行される。定型文。

### パッケージ
複数のモジュールをまとめるための仕組み。
