# FFmpeg

動画や音声をいじるライブラリ。
C言語等から使えるライブラリの他、動画・音声変換器`ffmpeg`、簡易動画・音声プレイヤー`ffplay`、動画・音声の情報を表示する`ffprobe`の3つのコマンドラインツールが提供されている。

- [FFmpeg](https://ffmpeg.org/): 公式サイト
- [Documentation](https://ffmpeg.org/documentation.html): 公式ドキュメント
    - [ffmpeg tool and FFmpeg components](https://ffmpeg.org/ffmpeg-all.html): コマンドラインツール`ffmpeg`と各種コンポーネントのドキュメント

## ffmpeg（コマンドラインツール）
### 時間で切り抜き
- [ffmpeg で指定時間でカットするまとめ](https://nico-lab.net/cutting_ffmpeg/)

`[duration]`は秒数または`hh:mm:ss`形式で指定する。

```shell
$ ffmpeg -i input.mp4 -t [duration] -c copy output.mp4 # 最初から指定時間のみ切り出す
```
