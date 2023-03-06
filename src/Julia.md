# Julia
科学技術計算に特化した言語。

- 高速
- 多重ディスパッチによる容易な拡張
- 漸進的型付け

あたりが売り。
これらはLLVM上で動くJIT(Just In Time)コンパイラによって実現されている。

普通の数式のような書式でプログラムが書けるのが特徴で、識別子にUnicodeの大量の文字が使えたりする。

- [The Julia Language](https://julialang.org/): 公式
- [Julia 1.3 Documentation](https://docs.julialang.org/en/v1/): 公式ドキュメント
    - [Unicode Input](https://docs.julialang.org/en/v1/manual/unicode-input/#): JuliaのREPLではTeXのような記法+TabキーでUnicodeの諸々の文字が入力できる。このページはその一覧。単なる特殊文字キーボードとしてもJuliaのREPLは優秀である。
- [公式ドキュメント ver0.6 の和訳](https://hshindo.github.io/julia-doc-ja-v0.6/index.html)

## 幺(Yao)
量子計算シミュレータ。
「幺」という名前の由来は “Yao (幺) is the Chinese character for normalized but not orthogonal.”とのこと（[出典](https://docs.yaoquantum.org/dev/)）。

- わかりやすい中間表現をもつため量子回路を組みやすい
- 数ある量子計算シミュレータの中でも一二を争う高速さ
- 量子回路のパラメータに関する自動微分をいいかんじにやってくれる（ので変分量子アルゴリズムのシミュレーションに適す）

といいかんじ。Juliaに慣れてさえいれば使いやすそう。

- [Yao](https://yaoquantum.org/): 公式
- [Yao.jl: Extensible, Efficient Framework for Quantum Algorithm Design](https://arxiv.org/abs/1912.10877): 作者による解説論文
- [Yao.jl - Differentiable Quantum Programming In Julia](https://julialang.org/blog/2019/12/yao-differentiable-quantum-programming/): Juliaブログでの紹介

### 例：量子フーリエ変換（Quantum Fourier Transformation, QFT）
公式チュートリアルの[Quantum Fourier Transformation and Phase Estimation](http://tutorials.yaoquantum.org/dev/generated/quick-start/2.qft-phase-estimation/)より。

- 量子フーリエ変換というアルゴリズム自体については[量子フーリエ変換 (Quantum Fourier Transform) とは - 物理とか](https://whyitsso.net/physics/quantum_mechanics/QFT.html)がわかりやすい

```julia
using Yao

#=
量子フーリエ変換の回路qft(n)を作成する
qubitのindexは1-originであることに注意
小さな回路を組み合わせて作っていく
=#
A(i, j) = control(i, j=>shift(2π/(1<<(i-j+1)))) # i番目のqubitをcontrol qubitにしてj番目に位相回転を施す
B(n, k) = chain(n, j==k ? put(k=>H) : A(j, k) for j in k:n) # Pythonのリスト内包表記風にループさせられる
qft(n) = chain(B(n, k) for k in 1:n)

#=
外部ブロック(external block)に上記の回路をラップする
こうすると（上のような函数としての量子回路より）複雑なことが出来る
=#
struct QFT{N} <: PrimitiveBlock{N} end # PrimitiveBlock{N}のサブタイプとしてQFT{N}を作る
QFT(n::Int) = QFT{n}() # 構築用の函数を作る（ほとんどエイリアスでしかない）

# circuitという名の函数を作る
# 引数の型だけが必要なので::の前の変数名を略している
circuit(::QFT{N}) where N = qft(N)

YaoBlocks.mat(::Type{T}, x::QFT) where T = mat(T, circuit(x)) #QFT{N}型の行列を取得する函数を作る

# 以上のようにすると
# QFT(5)' のようにして回路の随伴がとれる（＝逆フーリエ変換が作れる）などの恩恵がある

#=
（量子でない）高速フーリエ変換による検算
=#
using FFTW, LinearAlgebra
# 普通の逆高速フーリエ変換で量子フーリエ変換はシミュレートできる
# apply!函数を作っておくと、ブロックが量子状態をパイプで受け取ったときにその回路を適用できる
function YaoBlocks.apply!(r::ArrayReg, x::QFT)
    α = sqrt(length(statevec(r)))
    invorder!(r)
    lmul!(α, ifft!(statevec(r)))
    return r
end

# 比較による検算
r = rand_state(5)
r1 = r |> copy |> QFT(5) # 逆高速フーリエ変換
r2 = r |> copy |> circuit(QFT(5)) # 量子フーリエ変換
r1 ≈ r2 # これがtrueを返すならOK
```
- 検算とは書いたが、このようにするとQFT(n)の函数を呼んでつくる量子回路は量子フーリエ変換ではなく高速フーリエ変換を行うことになる。これによってシミュレーションが高速化できる。
    - 上の例（チュートリアル）と同様のものが、実際にYaoExtensionsのQFTブロックとして提供されている（[リンク](https://github.com/QuantumBFS/YaoExtensions.jl/blob/master/src/block_extension/qft.jl)）。
