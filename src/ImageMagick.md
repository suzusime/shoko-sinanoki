# ImageMagick

画像変換をいろいろやってくれるコマンド／ライブラリ。

## 拡大縮小

- [Resizing or Scaling -- IM v6 Examples](http://www.imagemagick.org/Usage/resize/)
- [ImageMagick リサイズ補間アルゴリズム - Qiita](https://qiita.com/yoya/items/b1590de289b623f18639)

サイズは%のほかピクセル単位で指定できる。
`1280x720`だと、横1280px×縦720pxの _中に収まる最大の大きさにアスペクト比を保って_ 拡大縮小する。
`1280x720^`だと、横1280px×縦720pxの _範囲を埋め尽くす最小の大きさにアスペクト比を保って_ 拡大縮小する。
`1280x720\>`だと、横1280px×縦720pxの _中に収まる最大の大きさにアスペクト比を保って_ 縮小する（それよりも小さい画像の場合は拡大しない）。
`1280x720\<`だと、横1280px×縦720pxの _中に収まる最大の大きさに(?)アスペクト比を保って_ 拡大する（それよりも大きい画像の場合は縮小しない）。
`1280x720\!`だと、横1280px×縦720pxにアスペクト比を無視して拡大縮小する。

### 最近傍法
ドット絵をドット絵らしさを保ったまま拡大するならこれ。それ以外での使い道はあまり思い浮かばない。

```
$ convert input.png -sample 800% output.png
```

### 平均画素法
平均画素法（面積平均法）で拡大縮小。
イラストの拡大縮小をするとき、（よくあるバイキュービック法に比べて）主線がくっきり出るのでお気に入り。

```
$ convert input.png -scale 1280x720 output.png
```

### 様々な補間アルゴリズムを使う
`-resize`なら様々な補間アルゴリズムを指定可能。

```
$ convert input.png -filter Lanczos -resize 1280x720 output.png
```

指定できる補間アルゴリズムの一覧は

```
$ convert -list filter
```

で取得可能。


## 減色処理
256色以下に減色する処理はcolorsを用いて次のように書く。

```
$ convert input.png -colors 256 output.png
```

ただし、上のような指定は文字通り「256色以下にする」だけであり、色のパレットは任意の色から選ばれる。
パレットをいわゆる「8ビット色」の256色に固定するには次のようにPNG8形式で吐く（こうするとレトロな見た目になる）。

```shell
$ convert input.png PNG8:output.png
```

## 多ページ画像の分割
tiffなどは多ページを含むことができる。これの分割は次のようにすれば良い。

```
$ convert input_muti.tif output-%04d.png
```

ただし、上の方法はメモリが足りないと失敗するらしく、その場合は

```shell
END=2000
for ((i=1;i<=END;i++));do
echo $i
convert input_muti.tif[$i] -scene 1 output_$i.png
done
```

のようにループを回して1ページずつ処理すればよいらしい。

- [imagemagick - Creating and splitting large multipage TIFF images - Super User](https://superuser.com/questions/793219/creating-and-splitting-large-multipage-tiff-images)
