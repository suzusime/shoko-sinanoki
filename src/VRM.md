# VRM

ドワンゴやピクシブを初めとした会社による「VRMコンソーシアム」が提唱する「VRアプリケーション向けの人型3Dアバター（3Dモデル）データを扱うためのファイルフォーマット」。
既存の3DモデルデータをVRのためのアバターとして用いるためには「制作者によるデータ制作作法の違い」「制作ソフトウェアに依存するファイルフォーマットごとの情報量の違い」「配布する際のライセンス」等の問題があったため、これを解決するために提案されたのがこのVRM規格である。
既存の3Dモデル規格glTF2.0をベースとして拡張を施したもの。

## リンク集
- [VRM](https://vrm.dev/): 公式
- [VRMコンソーシアム](https://vrm-consortium.org/)
- [VRM - がとーしょこらのWiki的メモ帳](https://gatosyocora.memo.wiki/d/VRM)
    - すごい

## ビデオ通話でVRMアバターを使う
仮想Webカメラ出力を持っているソフトを使うことで、Zoom等のビデオ通話で自分の映像の代わりにVRMアバターを映すことができる。

知る限りでは[3tene](https://3tene.com/)が良さそう。
Webカメラで自分を映すだけで頭の動きや表情を反映してくれる。

- この手のソフトではFaceRigが有名だが、VRMモデルをFaceRigのモデルに変換するのはかなり大変そう。
    - [【VRoid】3Dモデル(.vrm)をFaceRigにインポートする方法](https://www.cg-method.com/entry/facerig-import-3dmodel/): かなり大変そうなことをやっている記事

## VRoid
ピクシブによる、3Dモデルを誰もが簡単につくれる／遊べるようにするための企画。

- [VRoid](https://vroid.com/)

VRoidStudioは既存のモデルの数値パラメータをいじったりテクスチャを変更したりすることで簡単に3Dモデルをつくれるソフト。制作したモデルの著作権は利用者のものとなる。
VRM形式で出力できる。VRoidStudioだけでは弄れない部分もかなりあるが、そういう部分はVRMに出力した後に改変すればよい。

VRoidHubはVRoidをはじめとしたVRMモデルをアップロードできる共有サイト。

VRoidモバイルは、携帯電話アプリでVRoidのモデルを作れるソフトであり、VRoidStudioと異なり髪の毛や目のプリセットから選ぶという更に簡単な形式であるが、こちらはモデルの権利がピクシブに帰属する上、VRMとしての出力もできない（VRoidStudioへのアップロードしかできない）。