# Perl
Perlはテキスト処理に秀でたスクリプト言語。
sedやawk、grepといったツールの代替として使うことで、環境依存性の少ないスクリプトが簡単に書ける。

歴史ある言語であるが、最近はRubyやPythonの勢いに吞まれている雰囲気。
Perlと比べるとPythonは全然違う方向性、RubyはPerlと同系統という感じ。
たとえばPerlの有力な使い途の一つにワンライナーがあるが、Pythonでワンライナーなどとても書けたものではないのに対し、RubyはコマンドラインオプションまでPerlに寄せていたりする。

Perlのオブジェクト指向は後付けであるため、最初からオブジェクト指向で作られた言語と比べると便利ではない（が、それはライブラリを作るのが難しくなるだけであって、単純にライブラリを使うだけなら難しくないという印象）。ただ、オブジェクト指向前提でない分ちょっとしたスクリプトを書くだけならシンプルに書けて良い気がする。

テストやドキュメントをきちんと書く文化がある世界であるらしく、こういった面も非常にありがたい。特に、言語機能からCPANで入れたモジュールまでほとんどのドキュメントが`perldoc`コマンドを打つだけで気軽に端末で読めるのには感動した。こういう環境が他の言語にもあってほしいと願うばかりである。

私はPerlを使うようになって日が浅いのだが、すぐに好きになってしまった。言語自体の魅力のためだけではなく、開発者ラリー・ウォールの文章に惚れ込んでしまったせいかもしれないが。

## リンク
- [The Perl Programming Language](https://www.perl.org/): 公式
- [MetaCPAN](https://metacpan.org/)
    - モジュール（ライブラリ）を検索できる
- [perldoc.jp](https://perldoc.jp/)
    - perldoc（マニュアル）の日本語訳
- [雅なPerl入門第3版](https://miyabi-perl.booth.pm/items/260345)
    - 同人誌。私はこれで入門した。良書。
    - Boothでは品切れになっているが、作者にTwitterで連絡すると買えるらしい。
- [2時間半で学ぶPerl](https://qntm.org/files/perl/perl_jp.html)
    - 他の言語を知っている人がPerlの文法を知るのに良いと思う。
- [はてな教科書](https://github.com/hatena/Hatena-Textbook)
    - はてなの新卒研修用テキスト。
- [Perl入学式](http://www.perl-entrance.org/index.html)
    - 0からプログラミングを学ぶ人にPerlを教える講座が開かれているらしい
        - 参加費無料らしくてすごい
    - [講義資料](http://www.perl-entrance.org/handout.html)が公開されている
- [Perl Hackers Hub：連載｜gihyo.jp … 技術評論社](https://gihyo.jp/dev/serial/01/perl-hackers-hub)
    - Perlまわりの（わりと新しめの）技術についての解説がされている連載。
    - Perlに限らない、ITの最近の流行の紹介という意味でも面白い内容が多い。

## インストール
ほとんどのUnix系環境に最初から入っている（OSの諸々のスクリプトがPerlで書かれている為）ので、軽く使うだけならばそれを使えば良い。

ただ、外部モジュールを入れようとか考え始めると逆にOSを壊さないか心配になってくるので、[plenv](https://github.com/tokuhirom/plenv) を使ってローカルにインストールするのがおすすめ。

モジュールのインストールには `cpanm` (cpanminus)を使うのが良い。plenvを使う際は `plenv install-cpanm` として cpanm をインストールする。

WindowsではWSLを使えば良いと思うが、[Strawberry Perl](http://strawberryperl.com/)のように Windowsネイティブで動くものもある。

## スクリプトの雛型
```perl
#!/usr/bin/env perl
use strict;
use warnings;
use 5.014;
use utf8;

say "Hello, world!";
```

とりあえずこれを最初に書いておけというおまじない。スクリプトはUTF-8で保存する。

ただ、これだとWindowsでコマンドプロンプトを使っている環境では文字コードまわりの警告が出る。
`Encode::Locale` というモジュールを入れて以下のようにしておくと、端末のエンコードに依存せず問題なく表示（及び入出力）できるはず。

- [ここ](https://gist.github.com/suzusime/c5b7f5f08bfc9b0c8a85d286bd094da1)に説明文つきで置いてある。

```perl
#!/usr/bin/env perl
use strict;
use warnings;
use 5.014;
use utf8;
use open ':encoding(UTF-8)';
use Encode::Locale;
binmode(STDIN, ":encoding(console_in)");
binmode(STDOUT, ":encoding(console_out)");
binmode(STDERR, ":encoding(console_out)");
Encode::Locale::decode_argv;

say "日本語表現";
```

- Encode::Localeに依存したくない場合は `binmode(STDIN, ":encoding(UTF-8)");`（UTF-8の場合）や `binmode(STDIN, ":encoding(cp932)");`（cp932≒ShiftJISの場合）のようにすればよい。

## ドキュメントの読み方
コンソールから`perldoc`コマンドでドキュメントが読める。

```sh
$ perldoc perl # 言語とドキュメントの概要
$ perldoc perlintro # 初心者向けの入門記事
$ perldoc perlrun # 'perl' コマンドのオプション等の解説
$ perldoc perltoc # 各ドキュメントの目次
```

`perldoc perltoc`はかなり詳細まで書かれているので、何を見ればよいかわからないときはまず`perldoc perl`の最初のほうに書かれている簡単な内容紹介を眺めるのがおすすめ。

- これらの日本語訳が [perldoc.jp](https://perldoc.jp/) で読める。更新日が古いことは多いが、Perl自体変化が少ないので十分役に立つ（翻訳者に感謝……）。

各種モジュールのドキュメントも同様に`perldoc`コマンドで読める。例えば`LWP::Simple`モジュールのドキュメントが読みたい場合は

```sh
$ perldoc LWP::Simple
```

で良い。

- [perldocべんりってはなし](https://qiita.com/karupanerura/items/53f0a5a8e38345f4ede3)
  - もう少しくわしい使い方の紹介
- [モダンPerlの世界へようこそ 第5回 Pod::Perldoc：ドキュメントはどこに？](http://gihyo.jp/dev/serial/01/modern-perl/0005)
  - 「Perlの場合，Googleのような検索エンジンに頼るのは本当に最後の手段でしかありません」といえるくらいドキュメントが充実しているのは、Googleが役立たないといわれはじめた現代となっては頼もしい。

### perldocの色付け
`Pod::Text::Color::Delight` というモジュールを入れた上で `perldoc -MPod::Text::Color::Delight perl`のように呼び出すと色がついてとても読みやすくなるのでおすすめ。

.bashrcなどに

```sh
export PERLDOC="-MPod::Text::Color::Delight"
```

と書いておくと自動で読み込んでくれる。

- [Pod::Text::Color::Delight というモジュールをリリースしました](https://moznion.hatenadiary.com/entry/2013/10/26/235833)
  - 作者による解説

### cpandoc
perldocはインストール済みのモジュールのドキュメントしか読めないが、cpandocというモジュールを入れるとperldocと同じ使い勝手でcpan上の任意のモジュールのドキュメントが読めるようになるので便利。

```sh
$ cpanm Pod::Cpandoc
$ cpandoc Task::Kensho
```

## コメント
```perl
# 1行コメント

=begin comment
複数行コメントはこのように begin coment と cut で挟む。
これは POD(Plain Old Documentation) というPerlのソースコードにドキュメントを埋め込む書式を利用したもの。
=cut
```

## ファイル入出力
```perl
# ファイルハンドルの作成
open(my $in,  "<",  "input.txt")  or die "Can't open input.txt: $!";
open(my $out, ">",  "output.txt") or die "Can't open output.txt: $!";
open(my $log, ">>", "my.log")     or die "Can't open my.log: $!";

# 一行ずつ読み込んで処理
while (my $line = <$in>) {
    chomp $line; #$lineには末尾の改行が含まれるのでそれを削除
    say "今読んだ行: $line";
}

# printやsayにファイルハンドルを渡すとそこに書き込める
print STDERR "This is your final warning.\n";
print $out $record;
say $log $logmessage;

# ファイルハンドルを閉じる
close $in or die "$in: $!";
close $out or die "$out: $!";
close $log or die "$log: $!";
```

## 外部コマンド（別プロセス）実行
**注意：ここではセキュリティのことを考えなくてもよい場合（信頼できる人だけが叩くスクリプトを作る場合）を扱います。CGIなど、不特定多数の人の入力をもとにコマンドを実行する場合はここに書かれていない諸々のセキュリティ上の注意が必要です。**

### system()
起動するだけなら `system()` でよい。

```perl
system("ls");
```

### 標準出力をとる
標準出力をとりたいならバッククオートを使う。

```perl
my $output = `ls`;
print "Output: $output";
```

### 標準出力といっしょにエラー出力をとる
バッククオートの中にリダイレクトを書けばよい。

```perl
my $output = `gcc 2>&1`;
print "Output: $output";
```

### 標準入出力を両方使う
この場合openではできないので IPC::Open2 を使う。

なお、標準エラー出力も扱いたい場合は IPC::Open3 を使う（IPC::Open2はIPC::Open3のラッパーである）。

```perl
use Encode::Locale;
use IPC::Open2;

# 両方向のストリームを開く
open2(*README, *WRITEME, "cat -"); # cat - は標準入力の内容をそのまま標準出力に返すコマンド

# 文字コードを指定（非ASCII文字を入出力する場合に必要）
binmode(README, ":encoding(console_in)");
binmode(WRITEME, ":encoding(console_out)");

# 標準入力に流し込み
say WRITEME "こんにちは";
say WRITEME "さようなら、\nまた逢う日まで";
close WRITEME;

# 標準出力の結果を得る
while(<README>){
  print "> " . $_;
}
close README;
```

## 正規表現
### マッチ
`=~` を使えば正規表現マッチができる。

```perl
my $str = <STDIN>; #標準入力から入力
if($str =~ /hoge.+/){
  say "hogeから始まっています";
} else {
  say "hogeから始まっていません";
}
```

### 置換
`$str =~ s/前/後/` でできる。

```perl
$str =~ s/hoge/fuga/;
print $str;
```

非破壊で置換したいときは、`s///r`を使う。

```perl
my $new = $str =~ s/hoge/fuga/r;
print $str;
print $new;
```

## コマンドラインオプションの解析
標準ライブラリ Getopt::Long を使うのがお手軽。

- [Perlでコマンドラインオプションの解析に Getopt::Long を使う時、絶対に忘れてはいけない引数](https://tagomoris.hatenablog.com/entry/20120918/1347991165)
- [おそらくはそれさえも平凡な日々: 2013年のGetopt::Long](https://songmu.jp/riji/archives/2013/02/2013getoptlong.html)

```perl
use Getopt::Long qw(:config posix_default no_ignore_case);

GetOptions(
  \my %opt, qw/
    arch=s
    n=i
  / # sは文字列、iは整数
);

# 必須オプションの処理
if(!exists $opt{arch}) {
  die 'please set --arch=x86 or --arch=x86_64';
}

my $n = $opt{n} // 24; # 指定されなかったらデフォルト値を使う場合

# perl hoge.pl --arch=x86 --n=12
```

## Imager
画像を扱うライブラリ。
[[Perl/Imager]] 参照。

## GUI(Perl/Tk)
GUIライブラリにもいろいろあるが、一番お手軽なのはPerl/Tkだろうか。

- [Tk - metacpan.org](https://metacpan.org/pod/Tk#SEE-ALSO)
    - [Tk::UserGuide - Writing Tk applications in Perl 5 - metacpan.org](https://metacpan.org/pod/distribution/Tk/pod/UserGuide.pod#Perl/Tk-and-Unicode)

```
$ cpanm Tk
```
でインストールでき、

```perl
#!/usr/bin/perl -w 
use utf8;
use Tk;
use strict;

my $mw = MainWindow->new;
$mw->optionAdd( '*font' => 'ＭＳゴシック 12' );
$mw->Label(-text => 'こんにちは世界！')->pack;
$mw->Button(
    -text    => 'Quit',
    -command => sub { exit },
)->pack;
MainLoop;
```

といった感じで使える。

- [Perl/Tk ou pTk, une Fenêtre sur Perl](http://articles.mongueurs.net/magazines/linuxmag62.html)

## exe化
PAR::Packerで簡単にexe化できる。

- [pp - PAR Packager - metacpan.org](https://metacpan.org/pod/pp)
    - [PAR::Packer - PAR Packager - metacpan.org](https://metacpan.org/pod/PAR::Packer#COPYRIGHT)
    - [pp - Perl パッケージャ (Perl Packager) - perldoc.jp](http://perldoc.jp/docs/modules/PAR-0.75/script/pp.pod)

### インストール
Strawberry Perlなら

```
$ cpanm PAR::Packer
```

でインストールできる。
ネット上にはインストールに失敗するという記事も書かれているが、Strawberry Perl v5.28.2 + PAR::Packer 1.050の私の環境では、時間はかかったがテストもきちんと通りインストールに成功した。

### 使い方
```
$ pp -o hoge.exe hoge.pl
```
だけで1ファイルのexeになる。依存しているモジュールもModule::ScanDepsを使った解析によって自動で含めてくれる。
Perl/Tkなどを使ったGUIアプリで黒い画面を出したくない場合は `--gui` オプションをつければ良い。

なお、一時ファイルとして処理系を展開してから実行するらしく（参照: [Strawberry Perl(Portable)でPerlスクリプトのexeファイル化 | ビビビッ](https://vivibit.net/strawberryperl2exe/)）、exeの初回起動は少し時間がかかる。

## メールを送る
例えばGmailでメールを送るには以下のようにする（2020年10月現在）。

```
cpanm Email::Stuffer MIME::Base64 Authen::SASL Term::ReadKey
```


```
#!/usr/bin/env perl
use strict;
use warnings;
use 5.024;
use utf8;
use open ':encoding(UTF-8)';
use Encode::Locale;
binmode(STDIN, ":encoding(console_in)");
binmode(STDOUT, ":encoding(console_out)");
binmode(STDERR, ":encoding(console_out)");
Encode::Locale::decode_argv;

use Email::Stuffer;
use Email::Sender::Transport::SMTP;

# 本文
my $subject = 'Perlからのメール送信テスト';
my $body = <<'BODY_END';
これはサンプルめっせーじです！
なんとPerlからメールが送れちゃうんです！
BODY_END

# SMTPアカウント設定
my $sasl_password = prompt_for_password('Gmailのパスワード: ');
my $transport = Email::Sender::Transport::SMTP->new(
    host => 'smtp.gmail.com',
    ssl => 'starttls',
    sasl_username => 'example@gmail.com', # 送信元
    sasl_password => $sasl_password,
);

# 送信
Email::Stuffer
->from('example@gmail.com') # 送信元
->to('tim@example.com') # 宛先
->subject($subject)
->text_body($body)
->transport($transport)
->send;

# パスワードをユーザーに聞くサブルーチン
sub prompt_for_password {
    my $message = shift;
    require Term::ReadKey;
    Term::ReadKey::ReadMode('noecho');
    print $message;
    my $password = Term::ReadKey::ReadLine(0);
    Term::ReadKey::ReadMode('restore');
    print "\n";
    $password =~ s/\R\z//; # 改行の除去
    return $password;
}
```

- 何も設定せずにPerlのEmail系のモジュールを使うとローカルのSendmailサーバーに送信しにいくので、代わりにGmailのSMTPサーバーに送信するようにtransportの設定をしている。
- 上の例では sasl_password を直接スクリプトに書きたくないがために毎回ユーザーに聞くようにしている。これを直接スクリプトに書くならば`Term::ReadKey`は不要。
    - パスワードを聞くサブルーチンは[How can Perl prompt for a password without showing it in the terminal? - Stack Overflow](https://stackoverflow.com/questions/39801195/how-can-perl-prompt-for-a-password-without-showing-it-in-the-terminal)からもらってきた。
- Gmail側で「安全性の低いアプリ」の接続を許可する設定が必要かもしれない。
    - または2段階認証を有効化してアプリパスワードを発行してもよい（というかこちらが一般に推奨される）。
-  Email::Sender::Transport::SMTPのsasl_usernameのほうがGmailのアカウントのアドレスであり、Email::Stufferのfromには本来好きなアドレスを指定できる。が、Gmailの仕様でこのfromは無視される（＝受信側のメールボックスにはGmailアカウントのアドレスが表示される）ようである（指定したfromのアドレスは`X-Google-Original-From`というヘッダーに入ることになる）。
