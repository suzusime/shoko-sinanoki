# OpenType
MicrosoftとAdobeが共同で開発したフォントの規格。
以前はWindowsとMacとで使えるフォント形式が異なるという状況があったが、これにより解消された。

先進的なタイポグラフィを行うための機能を多く取り入れている。
多機能すぎて、フォントを描画するレンダリングエンジンの側がOpenTypeのすべての機能に対応しているとは限らないので注意。

- [OpenType® specification](https://docs.microsoft.com/ja-jp/typography/opentype/spec/)
  - Microsoftによって公開されているOpenTypeの規格書
  - OpenTypeが以前のTrueTypeとCFFを組み合わせたような規格であることから、たまにAdobeのPDFを参照せよと書かれていたりする。
- [ISO/IEC 14496-22:2019 “Open Font Format”](https://www.iso.org/standard/74461.html)
  - OpenTypeは “Open Font Format” という名前でISO/IECの標準にもなっている。
      - Microsoftの規格のほうが新しいものではある。
  - ISO/IEC Information Technology Task ForceのサイトからPDFがダウンロードできる。

## テーブル
フォントプログラムは一種のデータベースであり、OpenTypeでは種類ごとに**テーブル**という単位に分けてデータが格納されている。

テーブルは説明的な名前の他にそれぞれ4文字のタグをもつ。以下では `タグ (名前)` でテーブルを示した。
タグは必ず4文字であり、'CFF 'のようなものは最後にスペースが入っていることに注意。


### 必須なテーブル
フォントの種類（グリフのアウトラインの形式）によらず必須なもの。

- 'cmap' (Character to glyph mapping)
    - 情報交換に用いる文字コードから、フォント内部のグリフ番号(glyph index)（よく**GID**(Glyph ID)と呼ばれているが、規格書のcmapテーブルの解説にはこの単語は出てこない）に変換する写像
    - 複数の文字コードに対応できるよう、複数のサブテーブルを持てる
    - Unicodeの異体字セレクタを処理するのはこの層
- 'head' (Font header)
- 'hhea' (Horizontal header)
- 'hmtx' (Horizontal metrics)
- 'maxp' (Maximum profile)
- 'name' (Naming table)
- OS/2 (OS/2 and Windows specific metrics)
- 'post' (PostScript information)

### TrueTypeアウトラインのフォントに関係するテーブル
2次スプライン曲線で表現されるTrueType系のアウトラインを扱うためのテーブル群。

- 'cvt ' (Control Value Table (optional table))
- 'fpgm' (Font program (optional table))
- 'glyf' (Glyph data)
- 'loca' (Index to location)
- 'prep' (CVT Program (optional table))
- 'gasp' (Grid-fitting/Scan-conversion (optional table))

### CFFアウトラインに関係するテーブル
3次ベジェ曲線で表現されるCFF系のアウトラインを扱うためのテーブル群。

- 'CFF ' (Compact Font Format 1.0)
- CFF2 (Compact Font Format 2.0)
- VORG (Vertical Origin (optional table))

### SVGアウトラインに関係するテーブル
OpenTypeはSVGのグリフももつことができる。表示環境によってはアニメーションまでできる。

- 'SVG ' (The SVG (Scalable Vector Graphics) table)

### ビットマップグリフに関係するテーブル
OpenTypeにはアウトラインだけでなくビットマップのグリフを入れることもできる。それに関連するテーブル群。

- EBDT (Embedded bitmap data)
- EBLC (Embedded bitmap location data)
- EBSC (Embedded bitmap scaling data)
- CBDT (Color bitmap data)
- CBLC (Color bitmap location data)
- 'sbix' (Standard bitmap graphics)

### 先進的なタイポグラフィのためのテーブル
ただ文字を並べるだけではない、高度な組版を行うためのテーブル群。
日本語の場合、縦書きや異体字の扱いに必要であるため、「先進的」のイメージに反して日常的に必要になる。

- BASE (Baseline data)
- GDEF (Glyph definition data)
- GPOS (Glyph positioning data)
- GSUB (Glyph substitution data)
- JSTF (Justification data)
- MATH (Math layout data)

### OpenType Font Variationsに使われるテーブル
**可変フォント**(variational font)に関連するテーブル群。
少なくとも日本語フォントで目にする機会は少ないか。

- 'avar' (Axis variations)
- 'cvar' (CVT variations (TrueType outlines only))
- 'fvar' (Font variations)
- 'gvar' (Glyph variations (TrueType outlines only))
- HVAR (Horizontal metrics variations)
- MVAR (Metrics variations)
- STAT (Style attributes (required for variable fonts, optional for non-variable fonts))
- VVAR (Vertical metrics variations)

### 色付きフォント関連のテーブル
絵文字をはじめとした色付きのグリフを扱うためのテーブル。
各メーカーがそれぞれ規格をつくったために規格が乱立しているのが難点。

- COLR (Color table)
- CPAL (Color palette table)
- CBDT (Color bitmap data)
- CBLC (Color bitmap location data)
- 'sbix' (Standard bitmap graphics)
- 'SVG ' (The SVG (Scalable Vector Graphics) table)

### その他のOpenTypeテーブル
- DSIG (Digital signature)
- 'hdmx' (Horizontal device metrics)
- 'kern' (Kerning)
- LTSH (Linear threshold data)
- MERG (Merge)
- 'meta' (Metadata)
- STAT (Style attributes)
- PCLT (PCL 5 data)
- VDMX (Vertical device metrics)
- 'vhea' (Vertical Metrics header)
- 'vmtx' (Vertical Metrics)
