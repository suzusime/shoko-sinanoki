# Ruby

スクリプト言語の一つ。純粋オブジェクト指向。いろいろな点で非常に自由なのが特徴。
開発者はまつもとゆきひろ(Matz)氏。楽しくプログラミングできる(“enjoy programming”)ようにというのがモットー。

2013年以降、毎年クリスマスにメジャーバージョンアップ（2.xのxが上がる）されるのが恒例。

## リンク
- [オブジェクト指向スクリプト言語 Ruby](https://www.ruby-lang.org/ja/): 公式
- [オブジェクト指向スクリプト言語 Ruby リファレンスマニュアル](https://docs.ruby-lang.org/ja/latest/doc/index.html)
  - 公式リファレンス。このリンクは常に最新版に飛ぶ。
  - 他のバージョンのリファレンスを見たい場合は[こちら](https://docs.ruby-lang.org/ja/)
  - 検索サイト経由でリファレンスに辿り着くと、古いバージョンのものであることが非常に多い印象がある。バージョンをしっかり確認することをお勧めする。

## ファイル周り
### ファイル一覧の取得
```ruby
# jpgファイル一覧を取得
# globでは正規表現ではなくUnix的なglobで指定する
# /**/を挟むと任意の深さのファイルを取得できる
# fにはファイル名が文字列で入る
Dir.glob("./**/*.jpg").each do |f|
  # 拡張子を除いたファイル名のみ出力する
  if f =~ /.+\/(.+)\.jpg$/ then
    puts $1
  end
end
```

## xオプション
- [Rubyの起動 (Ruby 2.7.0 リファレンスマニュアル)](https://docs.ruby-lang.org/ja/latest/doc/spec=2frubycmd.html)

`ruby`コマンドに`-x`オプションをつけると、対象ファイルのうち「`#!`で始まり, `ruby`という文字列を含む行」以降「`OF`(ファイルの終り), `^D`(コントロールD), `^Z`(コントロールZ)または予約語`__END__`」までのみをrubyスクリプトとして実行できる。

例として、Windows向けのRubyInstaller2の`ridk`コマンドとして実行される[ridk.cmd](https://github.com/oneclick/rubyinstaller2/blob/master/resources/files/ridk.cmd)は、本質的に

```bat
@echo off

"%~dp0ruby" -x "%~f0" %*
@exit /b %ERRORLEVEL%

#!/mingw64/bin/ruby
require "ruby_installer/runtime"
RubyInstaller::Runtime::Ridk.run!(ARGV)
```

である。
これによりridk.cmdと同じフォルダにあるruby.exeに対して自身を渡し、自身の後半にあるRubyスクリプトを実行させている。

なお、xオプションをつけなくても「コマンドラインに指定したスクリプトが `#!` で始まるファイルで、その行に `ruby` という文字列を含まない場合、その行を読み飛ばします。`#!` に続く文字列が `ruby` という文字列を含む行を見つけたらその行以下を Ruby スクリプトとして実行します」という仕様があるため、

```bash
#!/bin/sh
exec ruby -x "$0" "$@"
#!ruby
p ARGV
puts "Hello, World!"
```

はxオプションなしでshのスクリプトとして実行できる。

- xオプションは他の言語にもある。Rubyが直接的に参照していそうなPerlでは、xオプションの説明として「メールのような大きな無関係のテキストのかたまりの中にプログラムが 埋め込まれている事を Perl に伝えます」とある（[perlrun - Perl インタプリタの起動方法 - perldoc.jp](https://perldoc.jp/docs/perl/5.26.1/perlrun.pod)）。

## Nokogiri
- [Readme - Nokogiri](https://nokogiri.org/)

HTMLやXMLを扱うライブラリ。WebスクレイピングやHTML/XMLの加工・編集が簡単にできる。
cssセレクタによる参照が（XPathを覚えなくて良いので）便利。
