---
layout: page
title: RubyのPost-Quantum Cryptography対応について、ついでにHPKE
date: 2026-06-23 18:00:00 +09:00
category: ja
---

タイトルの通りです。関ヶ原Ruby会議から帰ってきたらやろうと思っていたら **既に手をつけられていた** ので現状についてまとめることをしようと思います。ファイル名についていうと、関ヶ原の時点で何人かに「PQC対応入らないとRubyは勝手にオワコンになるよ、なので近く手をつけるよ」みたいな話をしていたことに由来しています。

## 前提: Post-Quantum Cryptography(ポスト量子暗号) is 何

現代の公開鍵暗号を支える素因数分解の困難性や（楕円曲線上のものを含む）離散対数問題は量子コンピュータの登場によって破られることが知られており、そのような性質に依存しない方式の暗号アルゴリズムが提唱され、アメリカのNISTは[FIPS 203(ML-KEM), 204(ML-DSA), 205(SLH-DSA)でこの標準化を行いました](https://www.nist.gov/news-events/news/2024/08/nist-releases-first-3-finalized-post-quantum-encryption-standards)。ML-KEMは鍵のカプセル化を行うKey Encapsulation Mechanismで、現在のDiffie-Hellman鍵交換（とそれをベースにするDHKEM）の後継となるもので、ML-DSA・SLH-DSAは電子署名、現在でいうECDSAなどの後継となるものです。

## 実は去年から対応していた

OpenSSL 3.5からEVP_PKEYに[ML-DSA](https://docs.openssl.org/3.5/man7/EVP_PKEY-ML-DSA/)と[ML-KEM](https://docs.openssl.org/3.5/man7/EVP_PKEY-ML-KEM/)が加わっており、これに合わせて去年の段階でSSLSocketがPQCのアルゴリズムを使えるように対応が入っています:

- [ruby/openssl #894 Adding post-quantum cryptography tests](https://github.com/ruby/openssl/issues/894)
- [ruby/openssl #913 ssl: add post-quantum cryptography (PQC) tests](https://github.com/ruby/openssl/pull/913)

なおSLH-DSAはOpenSSL上にあることにはあるのですが、「実装がまだ使える状態ではなさそう」（上記 [#894 へのコメント](https://github.com/ruby/openssl/issues/894#issuecomment-2935353393)参照）ということでテストケースには追加されていません。

## ユーザー側のAPI

EVP_PKEYのレイヤーでアルゴリズムが生えてるということはEVP_PKEYで動作するKEMのインターフェースがあればML-KEMはタダでついてくるはずです。これは最近になって [ruby/openssl #1062: Add EVP_PKEY KEM operations](https://github.com/ruby/openssl/pull/1062) で追加されました。これを使ってnet-sshでML-KEMを使った鍵交換に対応するpull requestが作られています（[net-ssh/net-ssh #1005: Add OpenSSL-backed ML-KEM key exchange](github.com/net-ssh/net-ssh/pull/1005)）。OpenSSHはpost-quantumでない鍵交換をするとwarningを出すようになりましたが、これが入るとRubyからも怒られずに使えるようになります。

またML-DSAもEVP_PKEYのレイヤーで追加されているということはEVP_PKEYのsign/verifyでタダでついてきそうですが、こちらも[昨年6/5のcommitでテストが追加されています](https://github.com/ruby/openssl/commit/e730e457cc00d240d9cafc60ba6a21793a420cc9)（[該当部分](https://github.com/ruby/openssl/blob/master/test/openssl/test_pkey.rb#L269-L286)）。

## PQCっていってもNIST指定のやつだけじゃないでしょ？

はい、それはその通りで、[Open Quantum Safe](https://openquantumsafe.org/)というプロジェクトがliboqsというライブラリでNIST指定以外のPQCアルゴリズムもサポートしており、これのRubyのバインディングとして[chrisliaw/roqs](https://github.com/chrisliaw/roqs)というものがあります。

## まとめ

**Ruby全く死んでなかった。** 去年の時点でOpenSSL 3.5にPQ対応入ったなーとは思ってた（これは[RubyKaigi 2025のスライドでも言及している](https://speakerdeck.com/sylph01/end-to-end-encryption-saves-lives-you-can-start-saving-lives-with-ruby-too?slide=80)）けどその時点で確認しておくべきだった。Ruby側にほとんど手を入れずにアルゴリズム名だけ指定すればある程度動いてくれるEVPすごい。

ちなみに #1062 では[OpenSSL::PKeyの `new_raw_public_key` / `new_raw_private_key` が使われており](https://github.com/ruby/openssl/pull/1062/changes#diff-ffc774d1e06edeb7b8bc54fbd11bc47e0f5283a3eccd03a6070cd9545905e3cfR293-R294)、これは[3年前に #646 にて追加した機能](https://github.com/ruby/openssl/pull/646)なのだけど、#1062 のauthorと #646 の元になった[PR #373](https://github.com/ruby/openssl/pull/373)のauthorは同じ人。

----

## おまけ: ruby/openssl版HPKE

[2.5年前に作業をしていたruby/openssl版HPKE](https://speakerdeck.com/sylph01/adventures-in-the-dungeons-of-openssl)ですが、[ちゃんとpull requestにしてmergeされました](https://github.com/ruby/openssl/pull/1061)[^1]。[gem版HPKE](https://rubygems.org/gems/hpke)との違いでいうと、OpenSSL版は **Base modeのみ対応**、gem版は **PSK, Auth, AuthPSKも対応**、というのがあります。OpenSSL版のスコープを限定したのはHPKEに依存している現行のRFC(MLS, ECH, OHTTP, ODoH)がBase modeのみを利用しているためです。Internet-Draftでいうと[COSE-HPKE](https://datatracker.ietf.org/doc/draft-ietf-cose-hpke/)がPSK modeを利用するためこれがRFCになった段階でPSK modeへの対応を検討します。一方でAuthモードについては[「正直やめようか」みたいな動きもある](https://github.com/openssl/openssl/pull/31336#issuecomment-4676958966)ので本当に必要になったタイミングでやろうと思います。

gem版でいいやと思っていた理由として「MLS作るにあたってはHPKEだけじゃなくてそのcipher suiteの中身も欲しい」という理由を挙げていましたが、実際MLSで必要なのは(one-shotの)HPKE **とHKDF** なので、MLSのsuiteからHKDFの種類を確定すればよく、[HKDF単体のAPIは既に存在するので](https://github.com/ruby/openssl/blob/master/ext/openssl/ossl_kdf.c#L248-L313)、[`melos` でHPKEとKDFのインスタンスを確定している部分](https://github.com/sylph01/melos/blob/main/lib/melos/crypto.rb#L119-L166)を全部OpenSSL-basedに書き換えることはできそう。

ついでにHPKEに関連するところでいうと、#1062 でKEMのAPIが生えたということはML-KEMだけでなくDHKEMもできるはずなので、[DHKEMのテストケースを追加するPRを出しました](https://github.com/ruby/openssl/pull/1069)。

[^1]: 番号を見ればわかる通り、「これ終わったらKEMのインターフェース足すか〜〜〜」と思っていたら #1062 が出てきてひっくり返った

----

