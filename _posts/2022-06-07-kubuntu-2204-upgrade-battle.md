---
layout: page
title: Kubuntu 22.04 upgrade battle
date: 2022-06-07 00:00:00 +09:00
category: ja
---

## これは何

Kubuntu 20.04から22.04にアップグレードした。

22.04が出たばっかりの頃にやったら `sudo apt autoremove` したらKDEがまるごと消し飛んでGUIブートしなくなるという現象が起こっていたが、現時点ではKDEがautoremoveされることもなくなり問題なくアップグレードできることがわかった。

以下、20.04から22.04にアップグレードするにあたって設定した点を書く。

## fcitxの設定

20.04から22.04にアップグレードするとデフォルトのinput methodがiBusになっている。Fcitxを使い慣れてるのでこっちにしたい。

- 何はともあれまずは `fcitx-diagnose`
    - そうすると `fcitx` はインストールされているけど立ち上がっていないことがわかる
    - そのためシステム環境設定からも「DBusがFcitxと接続できませんでした」という表示が得られる
- `im-config -n fcitx` でデフォルトのインプットメソッドを切り替え
    - `im-config` からGUIで設定してもよい
- ログアウトして再ログインするとFcitxが起動している
- ただしこの時点ではFcitxの設定GUIが起動しない。
- `sudo apt install fcitx-config-common fcitx-config-gtk`
    - KDEのfcitxのGUIの入れ方はわからなかった

## Electronアプリの動作がおかしい（クラッシュする）

特によく利用しているElectronアプリはKeybaseとAtom。調べる限りsandboxが悪さをしているらしい（？？？）のでそれを無効化すると問題なく起動する。

### Atom

タスクバーショートカットのコマンドを変更する。

- before: `env ATOM_DISABLE_SHELLING_OUT_FOR_ENVIRONMENT=false /usr/bin/atom %F`
- after: `env ATOM_DISABLE_SHELLING_OUT_FOR_ENVIRONMENT=false /usr/bin/atom --disable-seccomp-filter-sandbox %F`

参考:

- [https://askubuntu.com/questions/1405511/failure-to-launch-atom-on-ubuntu-22-04-lts](https://askubuntu.com/questions/1405511/failure-to-launch-atom-on-ubuntu-22-04-lts)
- [https://www.reddit.com/r/Fedora/comments/qm3qm1/comment/hj906bl/?context=3](https://www.reddit.com/r/Fedora/comments/qm3qm1/comment/hj906bl/?context=3)
    - `--no-sandbox` vs `--disable-seccomp-filter-sandbox`
    - [https://chromium.googlesource.com/chromium/src/+/0e94f26e8/docs/linux_sandboxing.md](https://chromium.googlesource.com/chromium/src/+/0e94f26e8/docs/linux_sandboxing.md) に詳しいこと書いてあるのであとで読む

### Keybase

KeybaseのGUIアプリはsystemd経由で起動するので、systemdの設定ファイルを書き換える。

`/usr/lib/systemd/user/keybase.gui.service` 内にて:

- before: `ExecStart=/opt/keybase/Keybase`
- after: `ExecStart=/opt/keybase/Keybase --disable-seccomp-filter-sandbox`

`disable-gpu-sandbox` でも起動する。

- [https://github.com/keybase/client/issues/24944](https://github.com/keybase/client/issues/24944)
- [https://github.com/keybase/client/issues/24688](https://github.com/keybase/client/issues/24688)

### Zoom

これも同様に `--disable-gpu-sandbox` あるいは `--disable-seccomp-filter-sandbox` が必要。ブラウザから `xdg-open` によって会議に入る方法ではこのオプション付きでは起動しないので、予めアプリケーションを開いておいた上で `xdg-open` することによって会議に入ることができる。
