---
layout: page
title: Corne Cherry ビルドログ
date: 2024-11-24 14:00:00 +09:00
category: ja
---

![](/images/crkbd.jpg)

[KeebWorld Conference 2024](https://keebkaigi.org/2024)が開催されるので、それにあたっていろいろ組み替えたのでメモです。

## 部品

- PCB: Corne Cherry
  - どのバージョンかは忘れたけどPCBが赤いやつ
- コントローラー: [SparkFun Pro Micro RP2040](https://www.switch-science.com/products/7441)
  - [コンスルーは高さ3.5mmのものが必要](https://www.switch-science.com/products/7448) （**これが本記事で最も重要**）
- キーキャップ: [Signature Plastics](https://www.solutionsinplastic.com/) のDSAキーキャップ
  - 最初に自作キーボードを始めたときから使ってるやつ
- キースイッチ: [Yushakobo Fairy Silent Linear Switch](https://shop.yushakobo.jp/products/5659)
  - 35g, 1.9mm, ファクトリールブ済み。軽量リニアスイッチしか勝たん
- ケース: IMK Corne R2
  - 前から持ってた

## やったこと

- はんだ付けをしたり組んだりすること自体は2019年くらいにやっていたと思う
- RubyKaigiが終わったあたりで[PRK Firmware](https://github.com/picoruby/prk_firmware) で動かしたいのでRP2040化したいとなった
  - ついでにSparkFun Pro Micro化するとUSB端子がType Cになって嬉しい！わざわざCorneのためだけにMicro Bを持ち歩きたくない
- 普通のPro Micro用のコンスルーがあったのでそれをはんだ付けしてみた
- しかし**[普通のPro Micro用のコンスルーだとUSB Type-Cの端子が高いので干渉する](https://x.com/s01/status/1847915602909736980)**
- はんだシュッ太郎で救出を試みるもコンスルーのプラスチックが溶けて基板と癒着したので無事死亡
  - RP2350版も同じように潰してしまうところだったがこちらはプラスチックが癒着してなかったのでシュッ太郎ではんだを処理したあとコンスルーとピンをペンチで剥がすことで多少裏面のランドが剥げる程度で救出することに成功した
- [高さ3.5mmのコンスルー](https://www.switch-science.com/products/7448) をスイッチサイエンスで買う
- こっちの高さなら干渉しない、装着してファームウェアを焼く
  - [ork_firmware](https://github.com/picoruby/prk_firmware)のReleasesから`.uf2.zip`を落として解凍、`.uf2`を書き込む
- [keymap.rb](https://gist.github.com/sylph01/0cdefeaf5fd1854cdd9f6f5e45e2a1cd) を書き込む
  - "PRK DRIVE"が出てきたら`keymap.rb`をドロップ
  - これは[prk_crkbd](https://github.com/picoruby/prk_crkbd)をベースにいつものキー配列に書き直しただけ
  - といってもいつものIrisのキー配列と記号の位置が微妙に違う気がするのでそこは修正の余地あり
- トラシュー
  - lowerレイヤーでの `KC_RSFT` が入らなかった
    - これは親の面に焼いていたキーマップではlowerレイヤーの右シフトが `KC_NO` になっていたのが原因。ただのキーマップ記述ミス
  - Macで `KC_HENK`/`KC_MHEN` がかな/英数キーとして認識されない
    - [キーコードの一覧](https://github.com/picoruby/prk_firmware/wiki/Keycodes_ja) を見ると
      - `KC_HENK` = International4
      - `KC_MHEN` = International5
    - これをKarabiner-Elementsで International4 → かな、 International5 → 英数 にマッピングする
    - そのうえで、Karabiner-Elementsのリマップが適用される範囲にPRKのキーボードが含まれるようにする
    - これをすると `HENK` / `MHEN` を使ってIMEのオンオフができるようになる