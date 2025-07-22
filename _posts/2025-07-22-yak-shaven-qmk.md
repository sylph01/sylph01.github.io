---
layout: page
title: 【yak shaving記録】QMK Firmwareの現状についていく
date: 2025-07-22 15:00:00 +09:00
category: ja
---

## これは何

タイトルの通りです。

初星学園の"Re;IRIS"は[KeebioのIris](https://keeb.io/collections/iris-split-ergonomic-keyboard)があまりにもよいのでリピートすることから名付けられたと知られていまして（いいえ）、

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">〽雨上がりを歌うIris 強い風に未来信じて <a href="https://t.co/2c2sCCQFss">pic.twitter.com/2c2sCCQFss</a></p>&mdash; sylph01 / G4きゅーぶ (@s01) <a href="https://twitter.com/s01/status/1947122947316932852?ref_src=twsrc%5Etfw">July 21, 2025</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script> 

もはや何枚目か覚えてない[^1]Iris（今回はロープロファイル対応のLM）を買ったのでQMKでキーマップを焼いたのですが最近のQMKは5年くらい前からだいぶ変わっているので少なくとも現時点でのセットアップ手順をメモしておきます。

[^1]: 最低でも2枚はおしゃかにしていて、手元に3枚あり、実家に1枚あり、最低2枚組み立ててないやつがあり、ということは最低でも8枚…？

## キーマップ作成

[QMK Configurator](https://config.qmk.fm/) で作る。

作った結果は [https://github.com/sylph01/qmk-json](https://github.com/sylph01/qmk-json) に入っている。

Irisには板が作られた時期によってrevisionが少しずつ異なる。私はrev6、rev7、rev8とLM-K[^2]の4種類を持っている。しかし最初にQMK Configuratorで作ったのはrev3向けのものであり、[rev3のJSON](https://github.com/sylph01/qmk-json/blob/master/iris-sylph01.json)をアップロードしてからConfigurator上で違うrevisionを選択するとキーマップを最初から作り直す羽目になってしまう。そこでrev3のJSONの `keebio/iris/rev3` ってなっている部分を `keebio/iris/rev6` に書き換えてアップロードしたら無事にrev6として認識された。revision間で異なるキーマップを設定したいわけではないのでそれぞれrev6〜8とLM-K(`keebio/iris_lm/k1`)向けのものを作成した。

[^2]: Iris LMにはKailhスイッチ向けのものとGateronスイッチ向けのものがある。ロープロファイルスイッチはメーカー間でピンのレイアウトが異なるのでこのようになっていると思われる

またレイヤーキーのタップアクションを変換・無変換キーに設定している[^3]のだが、 `KC_HENK` / `KC_MHEN` っていう名前だったのがいつの間にか違う名前に変わっていた。[QMKのキーコード一覧](https://docs.qmk.fm/keycodes_basic) を見ると `JIS Henkan` = `KC_HENK` は `KC_INT4`、 `JIS Muhenkan` = `KC_MHEN` は `KC_INT5` に変わっていたので

[^3]: なおLinuxデスクトップでこれらのキーをMacのJISキーボードみたいにIMEの有効無効に割り振るにはちょっと癖がいる。これは自分のOSセットアップガイドにメモしているのでそのうちどっかで書くかも

タップアクションを使っているというのが重要で、これの挙動がデフォルトで満足するならばQMK Configuratorでそのまま焼けばよいが、私はこれをしていたらタップアクションのmisfireが多発するので `TAPPING_TERM` をいじれる必要がある、となり、QMK Firmwareをソースビルドしてコンパイルする必要が出てきた。

## QMKのセットアップ

だいたい公式ガイドの[Setting Up Your QMK Environment](https://docs.qmk.fm/newbs_getting_started)に従うのだが、

- 素のpipで入れると `externally-managed-environment` が出るので `uv` 経由で入れる
    - `sudo apt install -y git python3-pip`
    - `curl -LsSf https://astral.sh/uv/install.sh | sh`
        - [https://docs.astral.sh/uv/](https://docs.astral.sh/uv/)
    - `uv tool install qmk`
    - `qmk setup`
- `qmk_firmware` の `users` 以下に `sylph01` ディレクトリを切り、[https://github.com/sylph01/qmk-json](https://github.com/sylph01/qmk-json) の `users/sylph01` の中身をコピー
    - キーマップ名に対応するグローバルコンフィグが設定できる。 `config.h` や `rules.mk` を `keyboards` 以下の個別ファイルにいちいち書かなくてよい
    - 私はこれは `TAPPING_TERM` を設定するために（のみ）使っている
        - レイヤーキーにタップアクションを割り振るのはIrisに限ったことではなく、他に使っている板でいうとcrkbdやPrime_eでも必要
- QMK Configuratorから落としたJSONをそのままコンパイルする: `qmk compile iris-rev6-sylph01.json` で可能。
    - **これJSON直接焼けることドキュメントに書いてあったっけ？**    
    - 中に書かれているキーボード名とキーマップ名を見て `-kb` や `-km` 相当を補って、JSONに書かれたキーマップの通りにファームウェアを作成する
- `qmk flash` も焼くファイルを直接指定できる
    - `qmk flash ~/qmk_firmware/keebio_iris_rev6_sylph01.hex`
    - rev8の場合はこれをやる必要がなく、 `.uf2` ができるので、それをUSBマスストレージクラスとして認識されたRP2040に対してダウンロードする
        - Irisシリーズだとrev6、rev7、LM-Kは `qmk flash` で焼く必要がある
    - 石がbootloaderモードで詰まって `dfu-programmer` がエラーを吐き続けてflashできないことがある。諦めてMacに繋いでQMK Toolboxでやって解決した。

## ところで今回のビルドについて

- 板: Iris LM
    - ケースはBlack Aluminum
- キースイッチ: Kailh Low Profile Deep Sea Mini Islet (Silent Linear)
- キーキャップ: PBTfans Umbra
    - 40s Kitが必要。高さ揃えてキーを埋めるだけならもしかしたら要らない…？
- ケーブルはAmazonで買ったテキトウなC to C
- [MagSafe Tenting Stand Kit](https://keeb.io/products/magnetic-magsafe-tenting-stand-kit-for-split-keyboard-r2) を買ったので家で使うときはそれを使って角度をつけている

----