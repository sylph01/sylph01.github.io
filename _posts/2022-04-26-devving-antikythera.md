---
layout: page
title: Antikythera Frameworkそのものの開発環境作成
data: 2022-04-26 08:30:00 +09:00
category: ja
---

## これは何

先日（だいぶ前）[Antikythera Framework](https://github.com/access-company/antikythera)に[contributeをした](https://github.com/access-company/antikythera/commit/d1148d27af3eb2da4b6d9d122e33f82ff7c2fc6c)。ACCESSを離れてから正式なルートでcontributeしたのが初めてだったが、（関係者に頼らない限りにおいて）開発環境構築の手順がそんなに自明でないのでメモとして記述する。需要が極めて限定的なのは知ってる（ほとんど私が必要になったときに思い出す用）。

## ある程度まではドキュメント通り

[Contributing to the antikythera code 以下 Developing](https://github.com/access-company/antikythera/blob/master/CONTRIBUTING.md#contributing-to-the-antikythera-code) に従えばとりあえずテストを動作させるまでの手順は書いてある。

- 以下のレポジトリ(すべてpublic repositoryなので関係者以外でもできる)をcloneする
    - [access-company/antikythera](https://github.com/access-company/antikythera)
    - [access-company/antikythera_instance_example](https://github.com/access-company/antikythera_instance_example)
    - [access-company/testgear](https://github.com/access-company/testgear)

## Antikythera instanceの用意

**ここが一番自明でない。**これをしないとサーバーを立ち上げてブラウザからアクセスするという手順ができず、ローカルでWebフレームワークとして動作させるというWebフレームワークとして一番大事なことができない。

- `antikythera_instance_example` の `mix.exs` に以下を記述
    - `antikythera_dep = {:antikythera, [github: "sylph01/antikythera", ref: "2479a8d8cad17c63d000782b5c0e432ade970aca"]}`
        - この例はGitHubの `sylph01/antikythera` レポジトリの該当のコミットハッシュを利用せよ、としている
    - ローカルのAntikytheraレポジトリのバージョンを利用して動作させる場合 `antikythera_dep = {:antikythera, [git: "/path/to/antikythera", ref: "commit_hash_of_your_developing_version"]}`
        - `antikythera` レポジトリのpath、動作させるべきコミットハッシュのバージョンを指定する
- その上で `antikythera_instance_example` を `mix deps.get; mix deps.get; mix compile`
- `testgear` レポジトリ内で以下を実行
    - `ANTIKYTHERA_INSTANCE_DEP='{:antikythera_instance_example, [git: "/home/username/antikythera_instance_example"]}' mix deps.get`
    - `ANTIKYTHERA_INSTANCE_DEP='{:antikythera_instance_example, [git: "/home/username/antikythera_instance_example"]}' mix compile`
    - `ANTIKYTHERA_INSTANCE_DEP='{:antikythera_instance_example, [git: "/home/username/antikythera_instance_example"]}' iex -S mix`
    - ここまで行うと、指定したバージョンのAntikytheraを利用して動作するAntikythera instanceを使ってWebサーバーが立ち上がる
    - `http://testgear.localhost:8080/` で `testgear` にアクセス可能になる

[Pull Requestコメント](https://github.com/access-company/antikythera/pull/187#issuecomment-1018226135=)にもこの手順を記述した。

## おまけその1: 何を直したか

- [Issue](https://github.com/access-company/antikythera/issues/186)
- [Pull Request](https://github.com/access-company/antikythera/pull/187)

[SameSite cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie/SameSite)が明示的に指定されない場合 `SameSite=Lax` をデフォルトとする挙動がChrome 80、Edge 88以降導入されている（Firefoxはこの設定を手動で有効にする必要がある）。当時開発中の案件（→なので当時私は社外の人間だったが厳密には非関係者ではない）において `SameSite=none` を明示的に必要とする挙動があったがこの設定が当時のAntikythera FrameworkのSession storeでは行うことができなかった。このpull requestはSession storeのplugに対してSet-Cookie optionを渡すことができるようにするものである。

現在のところAntikythera FrameworkのサポートするcowlibのバージョンがサポートしているSet-Cookieの値に限られるため `SameSite` の値を変更できるようにはなっていないが、 `Max-Age` の値が明示的に設定できるようにはなった。

## おまけその2: rant

実際このAntikythera instanceを用意する部分が全く自明でないせいでAntikythera Framework自体をオープンソースしていてもACCESS社外の人間が誰も使っていない・使えないという状況になっている気がする（→ヒント: 関係者は社内環境向けにカスタマイズされたAntikythera instanceを使っているためあまりその問題を意識せずに済んでいる）。[gearの開発＝Webサービスそのものの動作の開発についてはドキュメントがある程度整っている](https://hexdocs.pm/antikythera/gear_developers.html)のだが、肝心のフレームワークそのものの動作についてundocumentedな部分が多い（事実instance administrators向け文書はTBDで長らく放置されている）のでは正直オープンソースにしている意味はそんなにないのではないか？

…いやまあオープンソースになっているおかげでproject-specificでない修正を世の中に向けて出すことができたという側面はあるのだが…。あとAnkythera instanceのセットアップについては私もそこまで詳しいわけではない ~~のと政治的に尻込みする理由がある~~ ので多分私のほうで何かするかっていうとしない/できない。
