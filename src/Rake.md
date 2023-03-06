# Rake
ひとことで言うとRubyのMake。
更新を検知して依存関係に基づきファイルを生成するという伝統的なMake式のビルドツール。
なのだが、ファイル生成以外にもtask（MakeのPHONYターゲットに相当する）もあり、かなり汎用的に使える。「かなり汎用的」というか、Rakeの定義ファイル（Rakefile）はRubyスクリプトなのでRubyでできることは何でもできてしまう。
日常的な処理の手軽なCLIインターフェースとして優秀だと思う。

## リンク
- [library rake](https://docs.ruby-lang.org/ja/latest/library/rake.html)
  - 公式ドキュメント。正直あまり親切ではないと思う。
- るびくる＆RBのRubyプログラミング大作戦！ ファイルを扱う作業をRakeで便利にしよう！
  - [パート1：概要編](http://rubicle.net/rubicle_talk_1-1.html)
  - [パート2：実践編1](http://rubicle.net/rubicle_talk_1-2.html)
  - 初っ端から「好きな携帯電話メーカーはカシオ。無骨な見た目がステキだから。」とか出てくる時点でめちゃくちゃ古いことがわかるが、Rakeの基本は変わっていないのでわかりやすい解説として役に立つ。
- [Rakeの基本的な使い方まとめ](http://unageanu.hatenablog.com/entry/20100829/1283069269)
- [Rake](http://www2s.biglobe.ne.jp/~idesaku/sss/tech/rake/)
  - 詳しい。これを読めばだいたいのことができるようになりそう。
- [Rake Documentation（アーカイブ）](http://web.archive.org/web/20140327053134/http://docs.rubyrake.org/)
  - Rakeの作者による解説。

## インストール
最近のRuby（Ruby 1.9以降）だと標準ライブラリとなっているため、Rubyをインストールすればついてくる。
