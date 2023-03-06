# Perl/Imager
Imager - Perl extension for Generating 24 bit Images

>  Imager is a module for creating and altering images. It can read and
    write various image formats, draw primitive shapes like lines,and
    polygons, blend multiple images together in various ways, scale, crop,
    render text and more.

Perlの画像処理ライブラリ．要するに ImageMagick みたいなやつ．
ドキュメントがしっかりしていていいかんじ．
ImagiMagickよりインストールはたぶん楽．

## ドキュメント
```sh
$ perldoc Imager # ドキュメントの目次
$ perldoc Imager::Tutorial # チュートリアル
$ perldoc Imager::Cookbook # いろんなことをする例
```

## サンプル
### 基本形
```perl
#!/usr/bin/env perl
use strict;
use warnings;
use 5.014;
use Imager;

# 画像を新規作成
my $image = Imager->new(xsize=>256, ysize=>256);

# 四角形を書く
$image->box(xmin=>64, ymin=>64, xmax=>192, ymax=>192, filled=>1, color=>'gray');

# 画像を保存
$image->write(file=>'sample1.bmp')
    or die 'Cannot save image: ', $image->errstr;
```

- 新規作成された画像は既定では真っ黒になる．

### 既存画像を開く
```perl
#!/usr/bin/env perl
use strict;
use warnings;
use 5.014;
use Imager;

# 空の画像オブジェクトを作成
my $image = Imager->new;

# 画像を読み込み
$image->read(file=>'sample1.bmp')
    or die 'Cannot load image: ', $image->errstr;

# 画像を別名で保存
$image->write(file=>'sample2.bmp')
    or die 'Cannot save image: ', $image->errstr;
```
- この例では別名で保存したが，`read`してもファイルのロックはされないので同名で上書き保存もできる．
