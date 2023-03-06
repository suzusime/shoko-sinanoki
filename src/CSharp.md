C#
====

.NET上で動く言語。昔は「Windowsアプリを作るための言語」という感じだったが、最近はマルチプラットフォーム化に力を入れている。
C#という名はC++++から来ているらしいが、言語設計はC++というよりJavaに似ていると言われる。
似ているどころかJavaのパクリだと言われていたが、Javaが長らく機能追加しないうちにどんどんJavaにない機能を追加していった。結果としてかなり機能が多い言語になっている。オブジェクト指向言語であるが、いわゆる函数型言語から引っ張ってきた機能も数多い（Java方面ではScalaやKotolinのような“Better Java”的な言語が生み出されたが、C#ではC#本体に言語機能が追加されまくったためにあまりそういう話は聞かない）。

- [C# によるプログラミング入門 | ++C++; // 未確認飛行 C](https://ufcpp.net/study/csharp/) 
    - とりあえずこれを挙げておけばいいよねってなるサイト。
    - 入門コンテンツだけではなく新機能についても詳しい。
- [.NET API browser | Microsoft Docs](https://docs.microsoft.com/en-us/dotnet/api/) および[日本語版](https://docs.microsoft.com/ja-jp/dotnet/api/)
    - API（標準ライブラリ等）の検索サイト。

## .NET
.NETというのはC#などが動いているプラットフォームの総称。仮想マシンや中間言語、標準ライブラリが含まれる。
Microsoftが実装したものだけではなくいくつかの実装が存在し、それらの間の互換性は[.NET Standard](https://github.com/dotnet/standard)という規格により保証されている。

代表的な実装：

- .NET Framework
     - Microsoftによる伝統的な実装。当初は他の実装が存在しなかったため、今“.NET”と呼ばれているこのフレームワークの概念自体が“.NET Framework”と呼ばれていた。次世代の実装として .NET Core を開発したため、そちらへの移行が呼びかけられている。
- .Net Core
    - Microsoftによって新たに書き起こされたマルチプラットフォーム対応の実装。
    - 当初はWindowsのGUI周りのライブラリ（WinFormsとWPF）は含まれていなかったが、.Net Core 3.0にてWindows限定でこれらも使えるようになった。
- Mono
    - オープンソースでマルチプラットフォームな.NET実装

とまあいくつかあったのだが、2020年11月に出る **.Net 5** でこれらの実装は統一されるらしい（出典：[再統合された .NET:.NET 5 に関する Microsoft の計画](https://docs.microsoft.com/ja-jp/archive/msdn-magazine/2019/july/csharp-net-reunified-microsoft%E2%80%99s-plans-for-net-5)）。

.NET上で動く言語（CLI言語）はC#だけではない。Microsoftがおおっぴらに宣伝しているものでは他にVisual BasicとF#がある。他の言語は[wp: List of CLI languages](https://en.wikipedia.org/wiki/List_of_CLI_languages)を参照。

- [.NET のドキュメント | Microsoft Docs](https://docs.microsoft.com/ja-jp/dotnet/)

.NETの上の世界はガベージコレクタが走っており型検査ができるなど安全であることから、**マネージド**といわれる。一方、そうでない（C言語やアセンブリで動く）外界は**アンマネージド**とか**ネイティブ**とか呼ばれる。

## P/Invoke（プラットフォーム呼び出し）
- [ネイティブ相互運用性 - .NET | Microsoft Docs](https://docs.microsoft.com/ja-jp/dotnet/standard/native-interop/)

.NETの世界（マネージドな世界）からC言語等で書かれたライブラリ（アンマネージドな世界）の機能を呼び出すための機能。
.NETの機能なのでC#以外のCLI言語でも使える。

これを使いたくなる需要で大きなものはおそらくWindows APIの呼び出しである。本来P/InvokeするためにはC言語のヘッダに書くものに相当する宣言を書かなくてはならないのだが、Windows APIで必要になる宣言は[PInvoke.net](https://www.pinvoke.net/)にまとめられているため、これをコピペすれば良い。非常に便利。

Windows API以外、特にOSSのCのライブラリを使いたい場合は、P/Invokeに手を出す前にまず.NETのバインディングが存在していないか検索してみることをおすすめする。

## ASP.NET Core
### Blazor
今まではJavaScriptで書いていたクライアント（Webブラウザ）側の処理をC#で書けるようにしよう、というやつ。
適宜サーバーと通信するBlazor Serverと、全部クライアント側で処理するBlazor WebAssemblyがある。

以下は特に断りがなければBlazor WebAssemblyについて。
WebAssemblyで.NETの処理系を動かせばブラウザで.NETが動く、という面白方法論である。
最初の一回のロードが重いのが難点。

- [Blazor | Build client web apps with C# | .NET](https://dotnet.microsoft.com/apps/aspnet/web-apps/blazor)
- [ASP.NET Core Blazor の概要 | Microsoft Docs](https://docs.microsoft.com/ja-jp/aspnet/core/blazor/?view=aspnetcore-3.1)
- [Blazor Todo リスト アプリを構築する | Microsoft Docs](https://docs.microsoft.com/ja-jp/aspnet/core/tutorials/build-a-blazor-app?view=aspnetcore-3.1): 最も基本的なチュートリアル
- [ASP.NET Core SignalR を Blazor WebAssembly と共に使用する | Microsoft Docs](https://github.com/guardrex): サーバーとのリアルタイム通信はSignalRというのを使うらしい

#### サブディレクトリでアプリケーションを動かす
- [\[Blazor\] 404 on _framework/blazor.server.js with nginx reverse proxy · Issue #21775 · dotnet/aspnetcore](https://github.com/mkArtakMSFT)
- [Hosting Blazor Applications on GitHub Pages - MikaBerglund.com](https://mikaberglund.com/hosting-blazor-applications-on-github-pages/)
