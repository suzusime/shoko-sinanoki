# CMake
ビルド自動化のためのソフトウェア（ビルドツール）。
GNU Make や MSBuild のための設定ファイルを吐くという形で動作するので「メタ・ビルドツール」とも。
CMakeのCはC言語のCではなく、Cross PlatformのCらしい（[CMakeの昔のドキュメント](https://cmake.org/cmake/help/v2.8.9/cmake.html)のタイトルには "Cross Plarform Make" とある）。
C/C++以外の言語にも対応している。


## リンク
- [CMake](https://cmake.org/): 公式
- [実践C++応用講座CMake編](https://theolizer.com/cpp-school-root/cpp-school3/)
  - とりあえず最初はこれを読んでおけばいいのではないかと思える丁寧な解説。
- 勝手に作るCMake入門シリーズ（かみのメモ）
  - 1: [勝手に作るCMake入門 その1 基本的な使い方](https://kamino.hatenablog.com/entry/cmake_tutorial1)
  - 2: [勝手に作るCMake入門 その2 プロジェクトの階層化](https://kamino.hatenablog.com/entry/cmake_tutorial2)
  - 3: [勝手に作るCMake入門 その3 プロジェクトの設定](https://kamino.hatenablog.com/entry/cmake_tutorial3)
  - 4: [勝手に作るCMake入門 その4 外部ライブラリを利用する](https://kamino.hatenablog.com/entry/cmake_tutorial4)
- [An Introduction to Modern CMake](https://cliutils.gitlab.io/modern-cmake/)
  - 現代的なCMakeのベストプラクティスを紹介するサイト。章によって出てくる例のリポジトリ構成が異なっていたりするが、サンプルリポジトリは参考になる。著者は物理屋さんらしく、ROOT絡みの説明があったりする。

## 共有ライブラリのパスの追加
例えば `/usr/local/cuda/lib64` にある共有ライブラリ（動的リンクライブラリ）を使いたければ

```cmake
link_directories(/usr/local/cuda/lib64)
```

のようにする。

- [c++ - How do I add a library path in cmake? - Stack Overflow](https://stackoverflow.com/questions/28597351/how-do-i-add-a-library-path-in-cmake)
