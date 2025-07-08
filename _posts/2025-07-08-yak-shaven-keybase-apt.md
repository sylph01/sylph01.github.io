---
layout: page
title: 【yak shaving記録】Keybaseのapt updateがコケる問題
date: 2025-07-08 14:30:00 +09:00
category: ja
---

## これは何

タイトルの通りです。

## 背景

[Linux向けインストール手順](https://keybase.io/docs/the_app/install_linux) に従ってdebパッケージを入れた。

## 現象

(以下日本語にローカライズされたaptの表示)

```
エラー:3 http://prerelease.keybase.io/deb stable InRelease                                                   
  公開鍵を利用できないため、以下の署名は検証できませんでした: NO_PUBKEY 47484E50656D16C7
```

```
Warning: OpenPGP signature verification failed: http://prerelease.keybase.io/deb stable InRelease: 公開鍵を利用できないため、以下の署名は検証できませんでした: NO_PUBKEY 47484E50656D16C7
Error: リポジトリ http://prerelease.keybase.io/deb stable InRelease は署名されていません。
```

## 何をした

まず直接は関係ないけど

```
Notice: Some sources can be modernized. Run 'apt modernize-sources' to do so.
```

と出てたのでこれをした。 `[]` の中にパラメータを表記する旧記法からyaml-likeな記法に置き換わる。

```
Modernizing will replace .list files with the new .sources format,
add Signed-By values where they can be determined automatically,
and save the old files into .list.bak files.

This command supports the 'signed-by' and 'trusted' options. If you
have specified other options inside [] brackets, please transfer them
manually to the output files; see sources.list(5) for a mapping.
```

そうすると

```
Modernizing /etc/apt/sources.list.d/keybase.list...
- Writing /etc/apt/sources.list.d/keybase.sources
Warning: Could not determine Signed-By for URIs: http://prerelease.keybase.io/deb/, Suites: stable
```

みたいなことを言われた。

でKeybaseのcode signing keyを取ってきてkeyringに入れた。code signing keyのありかは [https://book.keybase.io/docs/server/our-code-signing-key](https://book.keybase.io/docs/server/our-code-signing-key) を参照。（以下の操作はこの鍵が信用できることを前提としている操作なので、ちゃんと理解した上で操作しましょう）（また調べる限り `/etc/apt/keyrings` 以下に置けっていう流派の人もいるらしいが、Tailscaleの鍵が `/usr/share/keyrings` 以下にいたのでこれを踏襲した）

```
curl -fsSL https://keybase.io/docs/server_security/code_signing_key.asc | sudo gpg --dearmor | sudo tee /usr/share/keyrings/keybase-keyring.gpg
```

teeしてる関係でめっちゃ文字化けした文字が表示されるがそんなもん。

これをやって、 `/etc/apt/sources.list.d/keybase.sources` を書き換える。具体的には最終行の `Signed-By:` のところに上記の `.gpg` ファイルのありかを書く。結果は以下。

```
Types: deb
URIs: http://prerelease.keybase.io/deb/
Suites: stable
Components: main
Signed-By: /usr/share/keyrings/keybase-keyring.gpg
```

Modernizeしてない場合は `[]` の中のパラメータの `signed-by=` を書き換える（か書き足す）。 `[signed-by=/usr/share/keyrings/keybase-keyring.gpg]` みたいになる。

その後 `sudo apt update`。無事更新されてることがわかる。

…これ素のインストールで発生したのでけっこういろんな人がハマるんじゃないかと思うけど、Keybaseについてはこの情報はGitHubにすらissue立ってないけど大丈夫そ？