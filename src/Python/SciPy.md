# SciPy
Pythonで数値計算するライブラリ群。
“SciPy”で指すものの範囲にはいろいろあるようだが、ここではNumPy、SciPy、MatPlotlib、Pandas、scikit-image、scikit-learn等をひっくるめて扱う。

- [SciPy.org — SciPy.org](https://www.scipy.org/): 公式

## インストール
### Windows
WindowsでSciPyを使いたい場合は，Anaconda（またはMiniconda）を使うのが良いと思われる．Python環境が既に入っている場合は気が向かないかもしれないが，自力でビルドする難易度を考えるとこちらのほうを推したい．

pipだけで簡単に入れるのは不可能．
というのも，SciPyは計算の高速化のためにFortranのライブラリを内部から呼び出しているのだが，インストールのためにはこれのビルドが必要でありこれがWindowsでは一筋縄ではいかないからである．私は5時間ほど費やしたところで諦めた．その点，Anaconda等はビルド済みのパッケージを配布してくれるので難しいことがない．

- NumPyだけを使うのであればpipでも普通に入る．が，これは高速化用のライブラリを使わないものになるので実行速度は劣る．

## ドキュメントの読み方
Jupyter や IPython のシェルで，ドキュメントを見たいもののあとに `?` をつけたものを実行するとドキュメントが読める．

```python
import numpy as np
np.linspace?

a="hoge"
a?
```

### Webのドキュメント
- [Getting Started](https://www.scipy.org/getting-started.html): 入門ドキュメントへのリンク
- [Scipy Lecture Notes](http://scipy-lectures.org/index.html)
  - Pythonで数値計算する方法の包括的な教科書．2019年版だとPDFで660ページもあった．
  - Pythonの基礎からNumPy，SciPy，Matplotlib，そしてscikit-learnまで触れられているみたい．

## 雛型
最初に次のように書いてインポートする．
`np`のような短縮形を使うところまでがお約束．

```python
import numpy as np
import matplotlib.pyplot as plt

# jupyter notebook を使うなら以下を足す
%matplotlib inline

# ipython ならこっち
%matplotlib
```

## 1次元配列
`np.ndarray` 型を使う．いろいろな生成方法がある．
ちなみにndarrayは N-dimensional array の意らしい（[出典](https://www.pythonlikeyoumeanit.com/Module3_IntroducingNumpy/IntroducingTheNDarray.html)）．

```python
# リストから生成
np.array([1.0, 2.3, 4.5, 1.9])

# 長さを指定した配列の生成
np.zeros(10) # 零埋め
np.ones(10) # 1埋め
np.empty(10) # 初期化しない

# 連番
np.arange(5) # => array([0, 1, 2, 3, 4])
np.arange(1, 3, 0.4) # => array([1. , 1.4, 1.8, 2.2, 2.6])
```
- [NumPyの配列ndarrayまとめ](https://qiita.com/nakasan/items/12ab3445a4f8c771f64e)
  - 詳しい．
- [NumPy配列のスライシング機能の使い方](https://deepage.net/features/numpy-slicing.html)
  - 配列のスライスについて。
  - （Python標準のリストと違い）スライスしても標準では新たな配列がつくられないことに注意。

## 最適化（フィッティング）
`scipy.optimize` を使う。

- [2.7. 数学的最適化: 関数の最小値を求める](http://www.turbare.net/transl/scipy-lecture-notes/advanced/mathematical_optimization/index.html)
  - 概要解説
- [Python SciPy : 大域的最適化アルゴリズム](https://org-technology.com/posts/scipy-global-optimization-routines.html)
  - basin hopping 等、大域最適化を行うアルゴリズムの紹介。

## Matplotlib
### 基本
```python
import numpy as np
import matplotlib.pyplot as plt

# 三角函数を描画
x = np.arange(0, 2*np.pi, 0.01)
plt.plot(x, np.sin(x))
plt.plot(x, np.cos(x)) # 繰り返しplotすると重ねて描画される

# 表示
plt.show()

# グラフ画像をファイルに保存
plt.savefig("gmpt.png")
```

### 体裁を整える
```python
import numpy as np
import matplotlib.pyplot as plt
plt.rc('text', usetex=True)

# 三角函数を描画
x = np.arange(0, 2*np.pi, 0.01)
# labelが凡例(legend)の表示になる
# label等には、$...$で囲むことでLaTeX記法が使える
plt.plot(x, np.sin(x), label="$\sin(x)$")
plt.plot(x, np.cos(x), label="$\cos(x)$")

plt.legend() # 凡例を表示

plt.title("This is title") # グラフのタイトル

plt.xlabel("$x$ dayo") # x軸の説明
plt.ylabel("$y$ dayo") # y軸の説明

# 表示
plt.show()
```

## Pandas
### 基本
2次元表のデータは「データフレーム」という形式で扱う。
いくつか作り方があるが、配列を用意して`pd.DataFrame`に辞書形式で渡すのが楽だと思う。

```python
import pandas as pd

# データの用意
hinmoku = ["蜜柑", "林檎", "葡萄"]
cost = [100, 85, 360]
area = ["愛媛", "青森", "長野"]
weight = [80, 120, 105]

# データフレームの作成
df = pd.DataFrame({"品目": hinmoku, "価格": cost, "産地": area, "重量": weight})
# >>> df
#    品目   価格  産地   重量
# 0  蜜柑  100  愛媛   80
# 1  林檎   85  青森  120
# 2  葡萄  360  長野  105

# csvへの保存
df.to_csv("fruits.csv")
```
