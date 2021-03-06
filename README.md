# 3d-pose-baseline-vmd

このプログラムは、[3d-pose-baseline](https://github.com/ArashHosseini/3d-pose-baseline/) \(ArashHosseini様\) を miu(miu200521358) がfork して、改造しました。

動作詳細等は上記URL、または [README-ArashHosseini.md](README-ArashHosseini.md) をご確認ください。

## 機能概要

- [OpenPose](https://github.com/CMU-Perceptual-Computing-Lab/openpose) で検出された人体の骨格構造から、3Dの人体モデルを生成します。
- 3Dの人体モデルを生成する際に、関節データを出力します
    - 関節データを [VMD-3d-pose-baseline-multi](https://github.com/miu200521358/VMD-3d-pose-baseline-multi) で読み込む事で、vmd(MMDモーションデータ)ファイルを生成できます
- 複数人数のOpenPoseデータを解析できます。
    - ~~2018/05/07 時点では正確に解析できません。1人のみの解析を試してください。~~
    - ver1.00(2019/02/13) で複数人数のトレースに対応しました。詳細は、[FCRN-DepthPrediction-vmd](https://github.com/miu200521358/FCRN-DepthPrediction-vmd) を確認してください。

## 準備

詳細は、[Qiita](https://qiita.com/miu200521358/items/d826e9d70853728abc51)を参照して下さい。

### 依存関係

python3系 で以下をインストールして下さい

* [h5py](http://www.h5py.org/)
* [tensorflow](https://www.tensorflow.org/) 1.0 or later

### H36Mデータ

3D骨格情報は、[Human3.6M](http://vision.imar.ro/human3.6m/description.php) に基づいて作成されます。 
以下より圧縮ファイルをダウンロードして、解凍後、`data`以下に配置して下さい。

[H36Mデータzip (Dropbox)](https://www.dropbox.com/s/e35qv3n6zlkouki/h36m.zip) 

### 学習データ

オリジナルの学習データは、Windowsの260文字パス制限にひっかかるため、パスを簡略化して再生成しました。
以下より圧縮ファイルをダウンロードして、解凍後、`experiments`以下に配置して下さい。

[学習データzip (GoogleDrive)](https://drive.google.com/file/d/1v7ccpms3ZR8ExWWwVfcSpjMsGscDYH7_/view?usp=sharing) 

## 実行方法

1. [Openpose簡易起動バッチ](https://github.com/miu200521358/openpose-simple) で データを解析する
1. [深度推定](https://github.com/miu200521358/FCRN-DepthPrediction-vmd)で 深度推定と人物インデックス別のデータを生成する
1. [OpenposeTo3D.bat](OpenposeTo3D.bat) を実行する
	- [OpenposeTo3D_en.bat](OpenposeTo3D_en.bat) is in English. !! The logs remain in Japanese.
1. `INDEX別ディレクトリパス` が聞かれるので、2.の`人物インデックス別パス`のフルパスを指定する
	- `{動画ファイル名}_json_{実行日時}_index{0F目の左からの順番}`
	- 複数人数のトレースの場合、別々に実行が必要
1. `詳細なログを出すか` 聞かれるので、出す場合、`yes` を入力する
    - 未指定 もしくは `no` の場合、通常ログ（各パラメータファイルと3D化アニメーションGIF）
    - `warn` の場合、3D化アニメーションGIFも生成しない（その分早い）
    - `yes`の場合、詳細ログを出力し、ログメッセージの他、デバッグ用画像も出力される（その分遅い）
1. 処理開始
1. 処理が終了すると、3. の`人物インデックス別パス`内に、以下の結果が出力される。
    - pos.txt … 全フレームの関節データ([VMD-3d-pose-baseline-multi](https://github.com/miu200521358/VMD-3d-pose-baseline-multi) に必要) 詳細：[Output](doc/Output.md)
    - start_frame.txt … 開始フレームインデックス([VMD-3d-pose-baseline-multi](https://github.com/miu200521358/VMD-3d-pose-baseline-multi) に必要) 
    - smoothed.txt … 全フレームの2D位置データ([VMD-3d-pose-baseline-multi](https://github.com/miu200521358/VMD-3d-pose-baseline-multi) に必要) 詳細：[Output](doc/Output.md)
    - movie_smoothing.gif … フレームごとの姿勢を結合したアニメーションGIF
    - smooth_plot.png … 移動量をなめらかにしたグラフ
    - frame3d/tmp_0000000000xx.png … 各フレームの3D姿勢
    - frame3d/tmp_0000000000xx_xxx.png … 各フレームの角度別3D姿勢(詳細ログyes時のみ)

## 注意点

- Openpose のjson任意ファイル名に12桁の数字列は使わないで下さい。
    - `short02_000000000000_keypoints.json` のように、`{任意ファイル名}_{フレーム番号}_keypoints.json` というファイル名のうち、12桁の数字をフレーム番号として抽出するため

## ライセンス
MIT


### 以下の行為は自由に行って下さい

- モーションの調整・改変
- ニコニコ動画やTwitter等へのモーション使用動画投稿
- モーションの不特定多数への配布
    - **必ず踊り手様や各権利者様に失礼のない形に調整してください**

### 以下の行為は必ず行って下さい。ご協力よろしくお願いいたします。

- クレジットへの記載を、テキストで検索できる形で記載お願いします。

```
ツール名：MMDモーショントレース自動化キット　/　MMD自動トレース　のいずれか
権利者名：miu200521358
```
- ニコニコ動画の場合、コンテンツツリーへ [トレース自動化マイリスト](https://www.nicovideo.jp/mylist/61943776) の最新版動画を登録してください。
    - コンテンツツリーに登録していただける場合、テキストでのクレジット有無は問いません。

- モーションを配布される場合、以下文言を同梱してください。 (記載場所不問)

```
配布のモーションは、「MMDモーショントレース自動化キット」を元に作成したものです。
ご使用される際には原則として、「MMDモーショントレース自動化キット」もしくは略称の「MMD自動トレース」の使用明記と、
ニコニコ動画等、動画サイトへの投稿の場合、コンテンツツリーもしくはリンクを貼って下さい。
　登録先：MMD自動トレースマイリスト(https://www.nicovideo.jp/mylist/61943776) の最新版動画
Twitter等、SNSの場合は文言のみで構いません。
キット作者が今後の改善の参考にさせていただきます。

キット作者連絡先：
　Twitter：https://twitter.com/miu200521358
　メール：garnet200521358@gmail.com

LICENCE

MMDモーショントレース自動化キット
【Openpose】：CMU　…　https://github.com/CMU-Perceptual-Computing-Lab/openpose
【Openpose起動バッチ】：miu200521358　…　https://github.com/miu200521358/openpose-simple
【深度推定】：Iro Laina, miu200521358　…　https://github.com/miu200521358/FCRN-DepthPrediction-vmd
【Openpose→3D変換】：una-dinosauria, ArashHosseini, miu200521358, kenkra　…　https://github.com/miu200521358/3d-pose-baseline-vmd
【Openpose→3D変換その2】：Dwango Media Village, miu200521358：MIT　…　https://github.com/miu200521358/3dpose_gan_vmd
【3D→VMD変換】： errno-mmd, miu200521358, kenkra 　…　https://github.com/miu200521358/VMD-3d-pose-baseline-multi
```

### 以下の行為はご遠慮願います

- 自作発言
- 権利者様のご迷惑になるような行為
- 営利目的の利用
- 他者の誹謗中傷目的の利用（二次元・三次元不問）
- 過度な暴力・猥褻・恋愛・猟奇的・政治的・宗教的表現を含む（R-15相当）作品への利用
- その他、公序良俗に反する作品への利用

## 免責事項

- 自己責任でご利用ください
- ツール使用によって生じたいかなる問題に関して、作者は一切の責任を負いかねます
