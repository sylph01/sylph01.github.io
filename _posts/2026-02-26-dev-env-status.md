---
layout: page
title: 開発環境現状確認2026
date: 2026-02-26 15:00:00 +09:00
category: ja
---

流行ってるので(cf: [開発環境現状確認](https://b.hatena.ne.jp/q/%E9%96%8B%E7%99%BA%E7%92%B0%E5%A2%83%E7%8F%BE%E7%8A%B6%E7%A2%BA%E8%AA%8D) on はてブ)。

![](/images/PXL_20260226_072106130.jpg)

## 基本的な考え

- **デスクトップLinuxであることに最適化している**
- デスクトップLinuxである以上何かの拍子にOSをクリーンインストールするということは発生するので、そのときのダメージを最小化するように、**デフォルトに近い状態で体に合うものを使う**
  - ブートストラップ手段はマニュアル化してあり、だいたい30分〜1時間で元の環境を取り戻せる
    - 多分これの大部分を自動化しようと思ったらNixOSなんだろうと思っているが…
- **可能な限りGUIを使う**
  - CLIのほうが速いものはそれを使うが、CLIコマンドに記憶容量を割いていない

## OS

- 生活の99%はKubuntuでやっている。
  - なお会社勤め時代の最後の2年も99%デスクトップLinux生活をしていたが、Windowsが必要だったのは国プロ関係[^1]で明にWord(docx)、Excel(xlsx)、PowerPoint(pptx)が納品仕様として指定されてたことへの対応くらいだった
- Adobeとrekordboxの動くUNIXとして知られるmacOSを使うことがある。
- Windows？ここは開発環境現状確認なのでゲーミングOSの話はしていないと理解している。
  - 一時期はe-TaxがWindowsでないとできないということがあったらしいが、freeeで書類を作成してAndroid版の申告アプリを使って（技術的には）問題なく確定申告できている。

[^1]: [TTC](https://www.ttc.or.jp/)の標準化調査の公募。これはいちおう頑張って調べたら公開情報

## エディタ

Visual Studio Code。以前はAtomだったがサポートがなくなったのでな…。最近はドキュメント作業が増えたのでMarkdown Preview MermaidとかMarkdown PDFとかのプラグインが入ってるのが特徴的か。あと極めて今更ながらDev Containersが便利で、おま環問題がなかったことになる。エディタが縛られるとはいえDockerを生で使うよりもbootstrappingがしやすい点がよい。
ターミナル上のエディタはVim、[10年前に書いたvimrcはこんなんらしい](https://github.com/sylph01/dotfiles/blob/master/vimrc)がサーバー上で操作するときに使うくらいなので通常はほぼカスタマイズなし。

## AI周り

- メインマシンにはOpen WebUIが立っていてローカルでqwen3-coderとかgpt-ossが動くようにしてある。インターネットにあまり出てほしくないような調べものはこれでやれる。
- ついにAIラッダイト野郎であることをやめてClaudeに課金を始めた。これはどっかで別途書くことになると思う。基本的にはVisual Studio Codeのプラグインから。
- ちなみにノート（後述）をMCPサーバーに食わせることは明確にファンでないのでやっていない。

## ターミナルエミュレータ

Konsole。初手で以下の修正をする:

- プロファイルを新規作成
  - Linux色
  - フォント: Hack 11pt
  - 背景色の透明度 20%
- 新規作成したプロファイルをデフォルトにする

これを調べるにあたってメニューバーの出し方を忘れてた。Ctrl-Shift-Mで出す。

## シェル

bash。唯一変わったことをやってるところでいうと、自分の管理しているサーバー環境とrootでの操作のときのユーザー名の色を変更している。サーバー環境では水色(cyan)、rootでは紫(purple)。

```bash
if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi
```

というのがUbuntuのデフォルトのbashrcに書かれているが、これの `32m`(green) のところを `36m`(cyan)、`35m`(purple) に変更している。

あとMacで便利だった `open` を`xdg-open` に割り当てることはしたりする。

```
open() {
  xdg-open "$1" 2>/dev/null
}
```

## ランチャー

特に無し。

## ウィンドウマネージャー

KDE。WindowsからもMacからもGNOMEからも失われた昔ながらの使い勝手とモダンなデザインを維持しているという点でこれ一択。というか近年のGNOMEが本当に合わない、任意のデバイスはタブレットじゃないんだぞ？

## フォント

特にいじっていない。10年以上前はフォントのオタクだったのになぜ？

## ブラウザ

Firefox。デフォルトでついてくるのと、一時期ChromeがデスクトップLinuxと相性がいまいちだったことがあったためメイン利用のものを乗り換えた。たまにChromeでしか動かないプラグインとかブラウザAPIとかがあったりしてChromeを使うことはある。拡張機能で入っているのは以下:

- 1Password
- Amazon Link Shortener
- Control Panel for Twitter
- Copy as Markdown
- Proxy SwitchyOmega 3
- SAML-tracer
- tab list
- はてなブックマーク

## キーボード

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">〽雨上がりを歌うIris 強い風に未来信じて <a href="https://t.co/2c2sCCQFss">pic.twitter.com/2c2sCCQFss</a></p>&mdash; sylph01 / G4きゅーぶ (@s01) <a href="https://twitter.com/s01/status/1947122947316932852?ref_src=twsrc%5Etfw">July 21, 2025</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

[KeebioのIris](https://keeb.io/collections/iris-split-ergonomic-keyboard)。初星学園の"Re;IRIS"はKeebioのIrisがあまりにもよいのでリピートすることから名付けられたと知られており（いいえ）、手元に3組、実家に1組、まだ組み立ててないPCBが2組ある。キーマップは以下で、[QMK Configurator用のJSONファイルはこちら](https://github.com/sylph01/qmk-json)。

![](/images/keymaps.png)

- 左親指がSpace、右親指がEnter
- その隣のキーがLayer切り替え＋タップで変換/無変換 = IMEのオンオフ
  - OS入れてインプットメソッド(Fcitx5/Mozc)入れたら変換/無変換にくっついているIMEのオンオフ以外の機能を全部切る
  - Macの場合Karabiner-Elementsでこれらのキー入力をIMEのオンオフに置き換える
- Left GUIはMacではCommandキーだが、LinuxではComposeキーとして使う。ヨーロッパ言語のアクセント記号を入力するのに使う
  - 一番使うのがCompose + `"` (= Lower + S)で `ä` `ö` `ü` などを入力できる
- 日本語話者かつ外来語を多用するので「ー」が入力できる `-` はトップレイヤーに置いている

QMKのセットアップとかについては[【yak shaving記録】QMK Firmwareの現状についていく](/ja/yak-shaven-qmk.html)を参照。

Iris以外でたまに使うのはPrime_e(40%のAlice式配列)と60%のAlice式配列のやつがあり、だいたいレイヤーの下に埋めているキーはIrisと似たような配列になる。

## マウス

[MX Vertical](https://www.logicool.co.jp/ja-jp/shop/p/mx-vertical-ergonomic-mouse)。充電できるのがでかい。[MX Lift](https://www.logicool.co.jp/ja-jp/shop/p/lift-vertical-ergonomic-mouse.910-006488)も使っていたことがあるが、加水分解で滅んできてMX Verticalに入れ替えた。自転車でコケて手首を故障する前からこれを使っているが、手首を故障してからはこれでないと長時間使用はかなりキツいし、キーボードも傾けて使っている。

Linux環境の場合[Input Remapper](https://github.com/sezanzeb/input-remapper)で親指側の「進む」「戻る」ボタンをPgUp/PgDnに割り当てる。Macだったら公式ドライバの機能かKarabiner-Elementsのどっちか。

## その他

### バージョン管理

基本Gitなのはそうとして、LinuxではGitKraken、MacではSourceTreeで操作をする。日常の99.5%の操作はGUIでできるし、そのほうが間違いがない。どうしてもできないか正確にやることが困難な操作だけCLIでやるようにしているが、

### 特筆すべき細かいセットアップ手順

#### 日本語フォルダ名を英語化する

```
sudo apt install xdg-user-dirs-gtk
LANG=C xdg-user-dirs-gtk-update
```

`ダウンロード` フォルダが `Downloads` フォルダに変わるので、あらゆる手順の前にやっておきたい。

#### Fcitx

- `sudo apt install fcitx5-mozc`
- Fcitxを再起動（システム環境設定からできる）
- Mozcを入力メソッドとして追加
- Mozcの設定画面に入る
	- HENK, MHENをすべて無効化
- システム環境設定→地域の設定→入力メソッド→グローバルオプション
	- 入力メソッドを有効: HENK
	- 入力メソッドをオフ: MHEN

### ノート

self-hostedな[Joplin](https://joplinapp.org/)を使っている。FOSSだしEnd-to-end Encryptedだしself-hostできるしで言うことがない。ここに至るまでに[Day One](https://dayoneapp.com/)→（Mac/iPhoneを脱するために）Evernote→（無料版の使い勝手が微妙になったので）Joplin、と移ってきた。というか今見たらDay OneってAndroid版出たのか…。

紙のノートもだいぶ使っている。パソコンに入力する前の下書きは基本的に紙のノート（B5のルーズリーフ）だし、あまり電子になってほしくない考えごとは紙のノート（無印の文庫本ノート）に書く。でこれらはだいたい年の終わりにスキャナーで電子化して、後者は暗号化して保存している。

#### 補: セルフホストについて

[JONSBO N2](https://www.jonsbo.com/en/products/N2Black.html) にN100のついたマザーボードとHDD2枚を入れ、OSとしてTrueNASを入れて、NASとして運用している。Tailscaleで外からもアクセスできるようにしている。高頻度で使っているself-hostedなアプリはJoplinの他には[Immich](https://immich.app/)と[Gitea](https://about.gitea.com/)があり、いちおうKavita（電子書籍ビューワ）とJellyfin（メディアサーバー; 私の用途は音楽のみ）が入っているがあまりライブラリのメンテができてないのでそんなに使ってない。

### 今のメインマシン

前に使っていたのは [2022年11月の記事](https://d.s01.ninja/entry/20221111/1668168000) を参照。これの中身は [JONSBO Z20](https://www.jonsbo.com/en/products/Z20Black.html) に入れ替えてWindows専用機（ルビ: ゲーミングマシン）になっている（参考: [WindowsのインストールにはWindowsが必要](/ja/need-windows-to-install-windows.html)）。2025年6月に、ローカルでLLMが動かせる程度のマシンに乗り換えるか、ということで新規に組んだ。このときは面倒なので固定資産登録をしない、つまり30万の壁がないつもりで組んでいる。結果としてめちゃくちゃいいタイミングで組んだことになった（RAMの取得価格は**2セット合計で71400円**だったと記録されている。今1セットで150000円するが…？？？）。

|part|name|spec|misc|update?|
|---|---|---|---|---|
|case|[Fractal Design Define 7 Solid](https://www.fractal-design.com/ja/products/cases/define/define-7/black/)||||
|cpu|[Ryzen 9 9900X](https://kakaku.com/item/K0001630331/)|12c24t, 120W, 4.4GHz||y|
|m/b|[X870E Nova WiFi](https://kakaku.com/item/K0001656275)|ATX, SATA x4, M.2 x5||y|
|ram|[CP2K48G56C46U5](https://kakaku.com/item/K0001579728/)|PC5-44800 48GB x2|2 sets of 48x2, total 192GB|y|
|pow|RM1000x 2024 Cybenetics Gold ATX3.1|||y|
|ssd|(2TB Gen4x4の余ってたやつを挿した)||||
|g/b|ZOTAC Gaming GeForce RTX 5070 Ti Solid SFF OC|VRAMは16GB||y|
|cpu cooler|ASUS Prime LC 360 ARGB|||y|

なおこれの組み立てでミスったと思ったのは、液冷ラジエーターに360mmのサイズのやつを選択したことで5インチベイの空間を使ってしまい、BD-Rドライブを搭載できなくなって外付け化せざるを得なくなったこと。音源の取り込みでCDドライブは無視できない程度に使うんですよ…。

### 業務用ノートPC

ThinkPad X1 Carbon Gen13。なんと発売当時は石が新しすぎてLTSであるUbuntu 24.04ではカーネルが対応しておらずカーネルパニックを引いたのでUbuntu 25.04で動かしている。USキーボード、軽い、いざとなったら中身をいじりやすい、という条件を満たすとなるとこのへんになる。あとLenovoさんは**税込30万を絶妙に下回る**セールをやってくれるのが完全にわかっている（なのでコイツは固定資産登録されており、基本的に業務用でしか使うことがない。ゲーミングOSを消し飛ばしてるんだから業務用だよね）。

![](/images/cb2542a41f709af0.jpg)

----