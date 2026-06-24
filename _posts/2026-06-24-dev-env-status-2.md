---
layout: page
title: 開発環境現状確認2026 ver.2
date: 2026-06-24 21:30:00 +09:00
category: ja
---

yak shavingは楽しい。

[前回記事はこちら。](/ja/dev-env-status.html)

## major update: セットアップ手順を自動化した

前回記事にて、

> - デスクトップLinuxであることに最適化している
> - デスクトップLinuxである以上何かの拍子にOSをクリーンインストールするということは発生するので、そのときのダメージを最小化するように、デフォルトに近い状態で体に合うものを使う
>   - ブートストラップ手段はマニュアル化してあり、だいたい30分〜1時間で元の環境を取り戻せる
>     - 多分これの大部分を自動化しようと思ったらNixOSなんだろうと思っているが…

みたいなことを書いた。そうしたら最近Rubyコミュニティで話題の[『シンプリシティ』](https://www.oreilly.co.jp/books/9784814401710/)を読んでいたところ [yadm (Yet Another Dotfiles Manager)](https://yadm.io/) というツールの存在を知った。これはホームディレクトリをGitレポジトリのように扱いdotfilesなどを同期することのできるツールである。また、それだけにとどまらず、dotfiles以上の設定をこなすための[Bootstrap](https://yadm.io/docs/bootstrap)という機能があるし、センシティブな情報をGitでやりとりすることを助けるための[暗号化機能](https://yadm.io/docs/encryption)もある。

そして前回記事からのUpdateとして、今の私はClaude Max 5xの課金ユーザーである。というわけで中性子星くらい重いｴｲｱｰｽを挙げて[^1]いろいろ聞きながら自動化を行った。

[^1]: 個人テンプレの一つ。[Twitterで検索するとまあまあ用例出てくる](https://x.com/search?q=from%253As01%20%E4%B8%AD%E6%80%A7%E5%AD%90%E6%98%9F)。なおass（ケツ）をｴｲｱｰｽと発音するのはaの部分を過度に強調する場合で、一般的な米語ではｴｧｰｽ、イギリスではｱｰｽくらいの発音をする。

やったことはだいたい以下である。

- `yadm` のレポジトリの中身（念の為privateにしてあるが、中身は以下で全部）
  - `.bashrc`, `.bash_logout`
  - `.gitconfig`
  - `.asdfrc`, `.tool-versions`: `asdf` 関連ファイル
    - これも『シンプリシティ』で言及されていた。これを機に `asdf` でも `mise` でもいいので `.tool-versions` を仕事のレポジトリにcommitしてチームで共有して許される程度になってくれ〜〜〜〜
  - `.vimrc`
  - `.xinputrc`: im-configが作ったやつ。といっても変わったことはほとんど何も書いてない
  - `.config` 以下
    - `konsolerc`: Konsoleの設定ファイル
    - `asdf-plugins`: `asdf plugin list` をこれにコピーしている。bootstrap段階で `asdf plugin install` するためのもの。
    - `systemd/user/keybase.gui.service.d/override.conf`: KeybaseはElectronアプリで、Wayland下でIMEがアプリまでたどり着かないことがあるので、そのための起動コマンドをoverrideするための設定。これも以前Claudeに作ってもらった。
    - `mozc/config1.db`: Mozcの設定。`HENK` / `MHEN` に関する挙動を殺す設定を入れている。注意としてこれはバイナリファイルでありテキストでdiffが見れる性質のものではない。
    - `joplin-desktop/.gitignore`: ここ以下のファイルを間違ってコミットすると厄介なことになると言われたのでgitignoreしてる
    - `fcitx5/config`, `fcitx5/profile`: Fcitx5の設定ファイル
    - `yadm/`
      - `bootstrap`: bootstrapスクリプト。実質本体
      - `encrypt`: どのファイルを暗号化するかを指定するファイル
  - `.local/share/` 以下
    - `konsole/`
      - `Linux.colorscheme`: 20%の透過を足した版。default-profile.profileから参照される
      - `default-profile.profile`: Konsoleのデフォルトプロファイル。konsolercから参照される
    - `yadm/archive`: 暗号化されるもの。暗号化されているのはJoplinの設定ファイル
  - `.ssh` 以下
    - `config`: `conf.d/` を参照しろ、みたいなことを書いてる
    - `conf.d/*`: 個人で管理しているサーバーのconfigが書いてある。用途別に3ファイルほど
    - **鍵ファイルはマシンごとに生成する**
      - いちおう`.config/yadm/encrypt` で暗号化指定して暗号化してレポジトリに突っ込んでsyncするという方法は存在するが、マシンごとにssh鍵を作って要らなくなったらrevokeする、というスタイルのほうが好み
- `bootstrap` でやってることの概要
  - `LANGUAGE= LC_ALL=C xdg-user-dirs-update --force`
    - `xdg-user-dirs-update-gtk` のインストールは要らない！
    - `LC_ALL=C` の指定がポイント。 `LANG=C` だけだと他のロケールより優先順位が低いので乗っ取られる。これのせいで今までコマンドライン版 `xdg-user-dirs-update` ではできてなかった。
  - `kwriteconfig6 --file kwinrc --group Plugins --key wobblywindowsEnabled false`
    - Kubuntu 26.04から「ふにゃふにゃウィンドウ」がデフォルト有効になるし、なんならこれは設定画面から切るのでは切れないのでコマンドで切る
  - `kwriteconfig6 --file kwinrc --group Windows --key TitlebarDoubleClickCommand Minimize`
  - aptレポジトリの追加
    - 1Password, Slack, VS Code, Google Chrome
  - apt install
    - `git autoconf bison build-essential libssl-dev libyaml-dev libreadline-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm-dev libgmp-dev rustc`: Rubyビルド用
    - `fcitx5-mozc`: 日本語入力するので
    - `libfuse2t64`: Joplinが必要としてる
    - `direnv`: 一部プロジェクトで使う、dotfilesに書いてあるので入れないと面倒
    - `jq`
    - `1password slack-desktop code google-chrome-stable`
  - Fcitx5の設定
    - だいたい `kwriteconfig6 --file kwinrc --group Wayland --key InputMethod /usr/share/applications/org.fcitx.Fcitx5.desktop`
    - これはFcitx5のインストール後に設定しないといけない。これで一度ハマった
  - Tailscale, Zed, uv, Claude Code, Joplinをそれぞれのinstall.shを使ってインストール
  - stable URL debなパッケージのインストール: Discord, GitKraken
  - asdfのGoバイナリをGitHub APIを使ってバージョンの確定をしインストール
    - asdfのpluginを `.config/asdf-plugins` を読んでインストールする
  - Fcitx5のリロード

## だいたい操作はこんな感じ

- OSインストール
  - だいたいここまで7分
- `sudo apt update && sudo apt -y upgrade && sudo apt -y install yadm gh`
  - だいたいここまで11分
- `gh auth login`
  - 他のログイン済みマシンからDevice flow (`https://github.com/login/device`) でログインする
  - Ed25519のssh鍵が作られる
- `yadm clone git@github.com:sylph01/didactic-waffle.git`
  - 該当レポジトリはprivateであることに注意
- `yadm bootstrap`
  - ここが長い。ここまでだいたい23分
- `yadm decrypt`
- 再起動して設定やアップデートの反映
- 1Passwordにログイン
  - 携帯でQRコードをスキャンが一番速い
- 1Passwordを使ってFirefoxにログイン
- Firefoxの1Passwordプラグインにログイン
  - 同、携帯でQRコードをスキャン
- Firefoxの1Passwordプラグインを使ってGoogleにログイン
- `tailscale up`
  - Googleアカウントでログイン
- Joplinの設定
  - マスターパスワードの設定を1Passwordを見てやる
  - self-hosted Joplinのサーバーのパスワードを1Passwordから入力
  - syncを行う
- Joplin上のセットアップログにEd25519公開鍵を記入
  - ここまでだいたい32分
- 他のマシンからsshしなければならないサーバーの `.ssh/authorized_keys` にEd25519鍵を記入

…というわけで操作の手数がかなり減って、40分以内にセットアップが完了するようになった。日本語入力関係の設定が安定して再現でき、アプリのインストールや最低限のログインまでやって40分なら上等で、その上でSlackのログインとかまでやって1時間程度で新しいマシンをほぼ完全に作業できる状態にできるのなら言うことがないでしょう。

## 追記: 微妙なハマりどころ

- Dolphin（KDEのエクスプローラー的なやつ）のサイドバーの「場所」は自動では日本語→英語にならない。これは `~/.local/share/user-places.xbel` というファイルに記述されていて、これをsyncするとUUID周りで厄介なハマり方をすることがあるので、一度削除した上でロケールに合わせて自動生成してもらうのがよいとのこと。なので全てのDolphinの窓が閉じた状態で `rm ~/.local/share/user-places.xbel` をしてからDolphinを起動する。

## 今後やろうと思ってること

- セットアップ自動化の不完全な部分
  - VS Code、Zedの設定のコピー
    - Zedに至ってはそこまで使い込めてない。ローカルLLM用という割り切りをしているのでローカルLLMの動くマシンであることが前提となるが、ぶっちゃけ2桁Bのモデルをまともに動かせるのはデスクトップマシン1台しかないので今のところそちらのSyncにそこまでこだわりはない…
  - 一方VS Codeは割とDaily Driverなので何かしら策を考えないといけない。いちおうProfileのexport→importでやってはいるが…
    - ローカルLLM周りの設定
  - 同上、Syncしてできることが少ないので今すぐのモチベーションがそんなにない
- 同一マシンでの作業環境の分離
  - 具体的にはローカルを含むLLMに、クライアント（※顧客の意味）ごとに異なる作業環境を割り当て、それぞれの作業環境内でしか操作できないようにしたい
    - [Workshop](https://ubuntu.com/workshop) かその実態であるLXD、コミュニティフォークであるIncusあたりがいいのでは、とClaudeが言ってた
  - そのついでに各コンテナから外に出る通信をモニタリングしたい
    - なんならnetwork-wideにmalwareが変な通信してるみたいなのを気付けるようになるとベスト
    - このへんの手順もFableが生きてたうちになんとなく聞いていた

LLMの力を借りてLinuxデスクトップのコーナーケースみたいなのをかなりケアできるようになってきたので実はLLM元年＝Linuxデスクトップ元年だったかもしれない。いやs01氏の歴史観では2018年に即位してからずっとLinuxデスクトップ天皇の世なので…。めちゃくちゃびっくりしたのは、ThinkPad X1 CarbonのThunderboltポートの片方だけ認識が不完全みたいなバグを調査するのをClaudeに手伝ってもらってたら「このdmesgをバグレポするといいよ、Linuxカーネルメンテナのこの人が多分Thunderbolt関係やってるよ」みたいなのを名指しで挙げてきたこと。そんなことある…？？？

----