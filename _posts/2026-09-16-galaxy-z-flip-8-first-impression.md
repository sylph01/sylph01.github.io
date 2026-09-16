---
layout: page
title: インターネットをやめるためにスマホを更新した
date: 2026-09-13 12:00:00 +09:00
category: ja
---

![](/images/galaxy-z-flip8.jpg)

タイトルの通りです。

## Pixel 8 → Galaxy Z Flip8

### motivation

- Pixel 8の購入から2年半程度経ってた
- 折りたたみにしようと思ったモチベーション
  - [@onk](https://x.com/onk)さんが折りたたみスマホを持ってたのがよさそうだった
  - 自転車を乗るときにポケット内のフットプリントが小さいほうが嬉しい
  - **画面が小さいのでインターネットをやめられる**
    - カバー画面でSNSを使わないようにできる
    - 通知を受け取るデバイスとしてはスマホが必要だけど普段はそれで十分

### スペック

- Pixel 8
  - [https://www.gsmarena.com/google_pixel_8-12546.php](https://www.gsmarena.com/google_pixel_8-12546.php)
  - 150.5mm x 70.8mm x 8.9mm, 187g
- Galaxy Z Flip8
  - [https://www.gsmarena.com/samsung_galaxy_z_flip8_5g-14803.php](https://www.gsmarena.com/samsung_galaxy_z_flip8_5g-14803.php)
  - folded 85.7mm x 75.4mm x 13.1mm, 180g
  - unfoloded 166.9mm x 75.4mm x 6.1 mm

## 設定


- 移行: Samsung Smart Switchでだいたいのファイルは移行される
- 設定方針: 今までの慣れもあるので可能な限りSamsungのfirst-party appに頼らずAndroid(Google)のfirst-party appでやりたい
- SIMカードの移行
  - IIJmioのeSIMを使っているので自動では切り替えられず、関連づいているEIDの切り替え操作が必要だった
- 設定アプリ
  - ホーム画面
    - ホーム画面グリッド: 5x6
    - アプリ画面グリッド: 5列
  - ロック画面とAOD
    - Now bar→Now brief: オフ
  - カバー画面
    - Now briefとお気に入りのアプリを表示: オフ
  - アプリ→標準アプリを選択
    - ブラウザ: Firefox
    - ウォレット: Googleウォレット
  - 日付と時刻→24時間形式を使用
  - キーボード
    - Gboardを入れる
      - 今までの慣れがでかい。Samsungキーボードは微妙にキーの配列が違う
    - 一般管理→キーボード→標準キーボードをGboardに変更
    - 言語→日本語（12キー）
      - 半角スペースの使用 on
      - 三状態キーボードの使用 off
      - フリックのみ on
        - ポケベル打ちを無効化する
    - 単語リスト
      - Gboardのユーザー辞書はここ。いろいろ足す（まだやってない）
- アプリ
  - 1Passwordにログイン
  - 各サービスにログイン
  - Tailscaleが必要なサービスの設定
    - Joplin
      - サーバー情報、E2EE key、同期を待つ
    - Immich
  - 通知
    - 通知が欲しいアプリが通知無効になっていないことを確認する
    - Thunderbird
      - アカウント設定→通知
      - フォルダーを管理→受信トレイ→**プッシュ同期するをon**、通知on
        - これをしないと15分に一度同期されるというが、15分で同期されて通知されたことがない…
    - Gmail
      - アカウントを選択→通知→受信トレイの通知
        - **ラベル通知をon**
        - これをしないと通知されない
      - スワイプの動作→既読または未読にする（両方）
  - 学マス
    - Google Play連携があったのでファイルのダウンロードを待つだけ
  - Microsoft Authenticator
    - 1年以上使った覚えない… とりあえず個人のMicrosoftアカウントのやつだけ移行
- カバー画面にいるアプリ
  - カメラ
  - Googleウォレット
  - 電話
  - LINE
  - Gmail
  - Immich
  - ギャラリー
  - Joplin
  - ウィジェット: 時計、カレンダー、ボイスレコーダー、Thunderbird受信トレイ

## 1st impression

- 確かにカバー画面だけでほとんどの用はカバーできるし結果ノイズが少ない
- 意外とカバー画面でもthird-partyアプリが動く
  - Joplinはread-onlyで使うならサイドバーで対象のノートブックを探すのがちょっとしんどいくらいでちゃんと機能する
  - 出先で旅程を見るために使いたいんですよね
- 最新世代だけあって学マスがストレスなく動く
- とはいえ最新世代だけあって高い。AndroidにしたのはiPhoneが高いからではなかったのか…？
  - まあそのiPhoneも結構信じられん値段になっているのだが…
