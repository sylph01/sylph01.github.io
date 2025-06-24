---
layout: page
title: WindowsのインストールにはWindowsが必要
date: 2025-06-24 20:30:00 +09:00
category: ja
---

## これは何

今まで使っていたPCの中身を新しいケースに動かしてWindows専用機に変えたのだけど、その際の諸々トラブルシューティングした話のまとめです。

![](/assets/20250621_060539496a.jpg)

## "Install driver to show hardware"

### 背景

- 今まではUbuntuとWindowsのデュアルブート環境で、それぞれ別々のSSDに入っていたが、片方だけを新しいケースの中で使いたい
- ブートローダーとWindows Boot ManagerはUbuntuの入っている側のSSDに入っているので、単純に片方を移植しただけでは使えない
- そうするとWindowsの入ってる側はWindows Boot Managerがないのでブートしない
- じゃあWindows Boot Managerを入れるために復旧モードで入る必要がある、USBからブートして復旧モードで入ろう

### 現象

[Windows 11のダウンロード](https://www.microsoft.com/ja-jp/software-download/windows11) からISOをダウンロードしてMacからそれをddすることでUSBを作ってインストール対象のマシンで起動したところ、 "Install driver to show hardware" という画面が出た。ドライバが足りないというので足りなさそうなドライバをダウンロードしてUSBメモリに焼いても「ドライバーのスキャン中にエラーが発生しました」と出る。そもそも何が足りないのかもよくわからない。

![](/assets/install_driver.jpg)

### 何をした

**WindowsでWindowsのインストールUSBメモリを作った。**

具体的には、 [Windows 11のダウンロード](https://www.microsoft.com/ja-jp/software-download/windows11) の「Windows 11 のインストール メディアを作成する」から `mediacreationtool.exe` をダウンロードし、それを実行することでインストール用のUSBメモリを作った。

なお私は他にWindowsの動くマシンを所有していないので、Windows Boot Managerが入ってる側のSSDを改めて挿し直してそこからWindowsを起動した。

なので、**WindowsのインストールにはWindowsが必要である。** なんとMacやLinuxで `dd` を試みたら失敗したのでWindowsで作り直したらうまく行ったという報告は[他にもある。](https://qiita.com/aliceundteles/items/2f261fa22233cd59408f)（確か英語圏のフォーラムでも見た気がする）なんで？？？でもこんなこといちいち追跡調査したくない。

もちろん、正確にはWindowsのインストールDVDとかベンダーが作ったインストールUSBとかを持っていればその限りではないが…。

## 残りの手順

### Windows Boot Managerのパーティションを作る

- 見たのはだいたい以下
    - [Qiita - Windowsブートマネージャーの再構築](https://qiita.com/fuyuaice/items/5b810732134ee43dee6e)
    - [EaseUS - BCDboot：ブートファイルのコピーが失敗した](https://jp.easeus.com/amp/partition-manager/failure-when-attempting-to-copy-boot-files.html)
        - ソフトのダウンロードへの誘導が邪悪すぎるQ&Aサイトなので正直あまりリンクしたくないが決定打はここだった
- 本当はパーティションのresizeをして500MBくらいのUEFI用のパーティションを作るのだが、なぜかKDE Partition Managerで見てもサイズが変更できなかったので、後ろにちょうど700MBほど空いていたのでそこを使うこととした
- 「コンピュータを修復する」→「トラブルシューティング」→「コマンドプロンプト」
- `diskpart` に入る
- `list disk`
- `sel disk 0` (該当のディスクが0である前提)
- `list vol`
- `sel vol X` (該当のパーティションがXである前提)
- `assign letter="F"` (空いてる文字を使う)
- `active`
    - やれと書いてあるページ多いけど私はエラーが出た
- ここで `diskpart` から `exit`
- そのあと `bootrec /RebuildBcd` をするとよいと書かれてる文書が多いが、これをやったら私はエラーが出て最後まで行かなかった
    - 「ブートファイルをコピーしようとしてエラーが発生しました」と出る
- `bcdboot c:\windows /s F: /f ALL`
    - ここの `c:\windows` はそこに前のWindowsのインストールがいる前提
    - `F:` はさっきassignした文字

これでリブートしたらWindows Boot Managerができてた。

### Windowsを入れたあと、なぜかファンが100%の速度で回り続けている

よくわからんので[Fan Control](https://getfancontrol.com/)というGUIを入れてそれに任せることにした。起動直後でこれが上がってくるまで爆速で回るしシャットダウン時も爆速で回るけど知らん。

## まとめ

WindowsよりデスクトップLinuxのほうがインストールは楽。どうやらドライバのチェックもインストール時にWindowsアカウント利用を強制するためにインターネット接続性を強制したいためのものらしいので、非標準な環境ではしょうもないエラーでインストールできないみたいなのが増える一方になっていくと思うとしんどい。ゲーミングOSのためになんでこんなにyak shavingせにゃならんのや。

ちなみに新しいケース(JONSBO Z20)にはだいぶ満足している。NAS用ケースを含めてJONSBOは2台目だが、小型ケースにパワーユーザーが求めるものを本当によくわかっている気がする。

旧ケースにセットアップした新しい中身のほうはほぼ何事もなく動いてるのでそちらについては特筆することなし。気が向いたら書きます。