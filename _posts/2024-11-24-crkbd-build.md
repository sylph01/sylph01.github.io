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
  - [prk_firmware](https://github.com/picoruby/prk_firmware)のReleasesから`.uf2.zip`を落として解凍、`.uf2`を書き込む
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

----

## 11/27追記: もう1枚をType-C化した

![](/images/crkbd_atmega32u4.jpg)

- 使用したのは[Amazonで遊舎工房が販売しているType-C版 Pro Micro](https://amazon.co.jp/dp/B0CV51GBLZ)
  - コンスルーがセットでついているが、**結局上記と同じ理由により3.5mmのコンスルーでないと干渉する** ので使ってない
  - その点でいえば、コンスルーをスイッチサイエンスで入手して、板自体は [FORIOTの3枚版](https://amazon.co.jp/dp/B0CW343TCS) でも別に問題はない気がする
    - Amazonで検索したところ高さ3mmのコンスルーは売ってるけど3mmでいけるかどうかは未検証。5mmもあって多少オーバーキル気味に見えるがあと2mmくらいならIMKのケースには十分収まる
- Type-Cの端子が下向きになるようにPro Microとコンスルーをはんだ付けする
- そして基板に装着…してみると、RAW/TX0のピンからPro Microの端までの長さが微妙に異なるので**右側にしか装着できない**
  - これはIMK Corne固有の問題で、サンドイッチ型ケースのCorneならここが干渉することはないと思われる
  - FORIOT版のPro Microでも画像見る限り干渉するような長さをしていそう
  - もしかしたらType-Cの端子が上向きになるようにコンスルーを装着（上下逆に装着）することでもう1セット空いてるピンの穴を使ってPro Microを装着できるのでは…？という気がしたが既に2枚とも同じ向きでコンスルーをつけていたので検証できなかった
    - 逆向きにつける場合、Type-Cのソケットの高さのせいでコンスルーの高さが足りないということは起きないので付属している高さ2.5mmのコンスルーでも大丈夫と思われる
- 仕方がないので左側はmicroBのPro Microを流用。microBとType-C両方でつなげるようになった
- こうするとType-C側につないだときに普通にファームウェアを焼くと左右逆に認識するので、EEPROMに極性（右か左か）を固定する方法を使った
  - [config.h, keymap.cはこちら](https://gist.github.com/sylph01/79571118f76666c9f94b94751e4e80f1)
    - これのうち、`config.h` の `#define EE_HANDS` がEEPROMを見て極性を判断せよ、と指示する部分
    - その他の設定について
      - `TAPPING_TERM` は130〜150くらいに設定している
      - LEDをはんだ付けしていないので `RGBLIGHT_ENABLE` はしてないけど元のやつを残してるだけ
      - あとのキーマップはほぼPRKで書いたやつと同じ
  - MacにQMKを入れる
    - `brew install qmk/qmk/qmk`
    - `qmk setup`
    - `brew install avr-gcc`
      - なぜかこれが `qmk setup` で漏れているので手で入れる
    - `qmk setup`
      - ちゃんと設定が完了していることを確認する
  - 実際に焼く
    - defaultキーマップに上記のファイルを上書きする方法でやった
    - 左側(microB) `qmk flash -kb crkbd/rev1 -km default -bl avrdude-split-left`
    - 右側(Type-C) `qmk flash -kb crkbd/rev1 -km default -bl avrdude-split-right`