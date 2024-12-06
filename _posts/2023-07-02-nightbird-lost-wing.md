---
layout: page
title: nightbird lost wing
date: 2023-07-02 13:00:00 +09:00
category: ja
---

## TL;DR

- Twitter、本格的にヤバそう
- ヤバそうなのでソーシャルネットワークと自律分散的なインターネットについて語りました
- 私はここにいます、という情報を残しておきます
    - 長文読みたくない人は「私はここにいます」の節まで飛ばしてください

## 本文

我々を夜遅くまで眠らせずにいた「鳥」は、翼を失い、墜ちた。

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">To address extreme levels of data scraping &amp; system manipulation, we’ve applied the following temporary limits:<br><br>- Verified accounts are limited to reading 6000 posts/day<br>- Unverified accounts to 600 posts/day<br>- New unverified accounts to 300/day</p>&mdash; Elon Musk (@elonmusk) <a href="https://twitter.com/elonmusk/status/1675187969420828672?ref_src=twsrc%5Etfw">July 1, 2023</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

2007年5月からTwitterをやっているのでアカウント開設から16年が経過していることになるが、その過程でフォロー数は3000、被フォロー数は2700にまで上っている[^1]。その中には私の所属している業界や私のやっている趣味の関係もあってヘビーユーザーもかなり多い。そのような利用方法をしていれば、1回のタイムライン読み込みで100個近くの投稿を読み込むことだってありうる。ということはホームタイムラインを10回くらい読み込んだらもうその日は使えませんということになってしまう。仮にTwitter Blueを買っていたとしてその10倍[^1p5]。通常時ならまあなんとかなるとして、カンファレンスの実況とかを追っていたら一瞬で尽きてしまう回数である。世界トップクラスのエンジニアが開発し、その中でも選ばれた人しか残っていないメンバーが開発しているサービス[^2]で、ログインして見える一番最初の画面が使い物にならなくなるとは誰が想像しただろうか？

## enshittification

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">一日600だか800だかしか読めないSNSで誰が広告見るんですか？</p>&mdash; ないさろーる (@nysalor) <a href="https://twitter.com/nysalor/status/1675217076745801729?ref_src=twsrc%5Etfw">July 1, 2023</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

プラットフォームの拡張と他のプラットフォームの淘汰のためにユーザーを甘やかし、競合がいなくなった時点でマネタイズのためにサービスを改悪するプラットフォーマーの振る舞いについて、[enshittificationという語が提唱されている](https://pluralistic.net/2023/01/21/potemkin-ai/)（日本語にあえて訳すなら「積極的クソ化」だろうか）。Twitterの買収の時点で大幅な赤字を抱えていてその黒字化が急務とされていたところに有料のユーザー認証[^3]と機能追加サービスであるTwitter Blueが発表されたが、その一方でTwitterの魅力を支えていた外部開発者によるbotやデータ分析を利用可能にするAPIが急速に制限され、サードパーティ製クライアントは使えなくなり、有益な情報を提供してくれていた自動投稿botはサービスをやめ、ついにはブラウザからの利用ですらAPI制限によってトップページが利用できないまでになってしまった。enshittificationを極めた結果目玉の機能すら無料ユーザーにとってクソの山なのであれば、誰が好き好んでそのプラットフォームにコンテンツを提供しようと思うだろうか？

Twitterを言論空間として見た場合に悪意のあるbotによるフェイクニュース拡散や影響力工作などが問題であるというのは確かにわかる。しかし、APIの有料化が実装された後も有害なスパムアカウントの活動は続いており[^4]、その対策も「相互フォロー以外へDMを送れるのはTwitter Blueのみにします」といったものの実態はフロントエンドでフィルターしてるだけ、というお粗末なものである。結局のところ言論空間としてのTwitterを守ろうとしているというのも考えすぎで、ただ絞れるところから金を絞り上げようとしているだけなのではないか？

もっとも、それを批判したところで「資本主義とはそういうものです」というのは現実で、それを受け入れた上でやっていくしかないのだが…。

## Twitterという空間が実現したもの

日本におけるTwitterの受容史とかを本格的に語るつもりはないしその能力はないが、テレビ番組の中でTwitterが言及される程度には日本の一般大衆に受容されたことで、Twitterアカウントをカジュアルな連絡先として交換し、ゆるくつながることができた、ということは自分の人生にとても大きな意味を持っている。私がフリーランスのWebエンジニアとして仕事を得られているのも同業者のネットワークがTwitterにあることに依るものが大きい。またTwitterはカジュアルなネットワークなので仕事上のネットワークだけでなく趣味のネットワークもその上に同居している。今の仕事の一部は仕事のネットワークと趣味（オーケストラ）のネットワークの結節点から発生していることを考えると、複数のネットワークがカジュアルに同居できる性質は自分の人生に非常に影響が大きい。また、「リアル世界」の趣味[^5]の範囲であればFacebook上にもネットワークは存在しているが、Facebookが実名をはじめ多くの個人情報を公開することが前提のコミュニティである以上インターネットでゆるくつながるために利用することは難しく、そのためこのようなネットワークは半匿名[^6]のコミュニティであるTwitterに頼らざるを得ない。日本人の大多数がTwitterアカウントを持っていることによって、実社会に基づく個人情報をすべてつまびらかにせずともただTwitterのアカウントを交換するだけで、ゆるく交流が開始でき、深い交流につながっていくことができた。現に、[地方に移住した今](https://d.s01.ninja/entry/20211016/1634374290)一番物理的に会っているのは音ゲーマーであるが、私はその大多数に実名をしゃべっていないしFacebookに書いているようなバックグラウンド情報をしゃべっていない。

みんながアカウントを持っていたカジュアルなネットワークというとmixiであった時代があった。大学時代にオーケストラで知り合った人や同時期にゲームセンターで知り合った人のソーシャルグラフはmixi上に築かれていた。mixiも自らの強みであった「招待制による安心感」「足あと機能による安心感やネットワークの拡大能力」を自ら潰すenshittification[^5p5]によりユーザー離れを起こしてしまった。API利用制限によって外部アプリケーションへの認証連携が危ぶまれたときや外部SNSへのリンクの言及の禁止が行われたときもActivityPubへの「移住」現象が見られたが、misskey.ioやmstdn.jpなどの大規模インスタンスが軒並み過負荷を起こしていることから見て、Twitterからの「移住」現象は今回のenshittificationの進行で新たなフェーズを迎えているように思う。mixiからTwitterに拠点を移したときに失ったソーシャルグラフもかなりあるが、今回のTwitterからのディアスポラによってまた多くの関係が失われてしまうのではないか？特にTwitterでは技術にそこまで明るくない人でもアカウントを持てたので、同業者以外とのつながりを一気に失ってしまうことに対する危惧が強い。

## 鳥の羽ばたく真の青空(True Blue)はBlueskyなのか？

<blockquote class="bluesky-embed" data-bluesky-uri="at://did:plc:zxcajoi4lpw6u7sq3lalz2kb/app.bsky.feed.post/3jyr57bzyyd2d" data-bluesky-cid="bafyreihlpwaicvhmne7mjvmk74mevjlxhpnviieo7k23pficpw3u374ijm"><p lang="">〽 What happens to a heart when love breaks?
Does it fade into a shadow of remorse, when it aches?
Or can it fly to the sky like a bird,
Lighter than ever, stronger too
Oh I think a heart always learns to come through</p>&mdash; sylph01 (<a href="https://bsky.app/profile/did:plc:zxcajoi4lpw6u7sq3lalz2kb?ref_src=embed">@sylph01.s01.ninja</a>) <a href="https://bsky.app/profile/did:plc:zxcajoi4lpw6u7sq3lalz2kb/post/3jyr57bzyyd2d?ref_src=embed">2023年6月22日 23:35</a></blockquote><script async src="https://embed.bsky.app/static/embed.js" charset="utf-8"></script>

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">True Blueのロング版、&quot;Opening my eyes, I see blue skies...&quot;という歌詞なんだけど、これはもしかしてTwitterが破滅した世界で「真のBlueとはbskyのことだった！」というテーマの歌</p>&mdash; sylph01 (@s01) <a href="https://twitter.com/s01/status/1667724815061639169?ref_src=twsrc%5Etfw">June 11, 2023</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

…とこのように冗談めかして書いているが、BlueskyがTwitterの後継になりうるかというと現状そのようには見えない。BlueskyもTwitterが抱えたマネタイズの問題を解決していない以上長期的にenshittificationの魔の手から逃れられないと思われる。負荷対策こそ一般的な技術なので業界の共有知として生き延びていくかもしれないが、bot対策・有害コンテンツ対策やマネタイズなどのプラットフォーム固有のノウハウは企業秘密として閉じ込められ失われていくだろう。

結局インターネットで長く生き延びているものは個人サイトでありEメールである。すなわち、自律分散的なシステムという、インターネットのあり方に忠実なものは多少形を変えても生き延びている。自律分散的なシステムは参入するのには技術がいる[^7]しそのコンテンツのリーチ力は大規模プラットフォームに比べると大幅に制限されるが、自律的に動かしている以上プラットフォーマーによるenshittificationの影響を受けることはない。その点でいえばActivityPubは自律分散的なサーバーの集合体である以上、個々のサーバーはネットワークから離脱することはあるかもしれないが、中央サーバーしかないサービスに比べれば総体としては生き延びるだろう。中央集権的プラットフォーマーによるenshittificationの時代には、原始的な個人ブログやテキストサイトが再びcoolなサイトとなるのだ。同様に、BlueskyがTrue Blueとなるためには、[The AT Protocol](https://atproto.com/)が複数のサービスによってサポートされ真に自律分散的なネットワークになっていく必要があるだろう。

自律分散性の観点でいうならば、私はmisskey.ioやmstdn.jpなどの大規模インスタンスにアカウントを持つことは控えており、これは私は多少技術を持っており自力でActivityPubネットワークに参加できる立場なので、そうでない人がネットワークに参加できるよう大規模インスタンスに1人分の余力を提供したほうがいい、と考えているからである。

## おわりに

確かにEメール(SMTP)は自律分散的なプロトコルであるけれど、正直SMTPにこれ以上生き延びられると困る(SMTPをやめろ)ので、次の時代のメッセージング・ネットワーキングのプロトコルを生み育てていければ、と思っており、今の私の大きな関心ごとの一つになっている。今回のTwitter離脱の波は今後のインターネット上のメッセージング・ネットワーキングについて再考する機会になったと思う。

## 私はここにいます

- Bluesky: [@sylph01.s01.ninja](https://bsky.app/profile/sylph01.s01.ninja)
- ActivityPub(Mastodon)
    - [Ruby.social](https://ruby.social/@s01)
        - 実質メイン。RubyコミュニティのCode of Conductに沿う範囲の用法で使用しています
    - [音ゲーマー丼](https://otogamer.me/@s01)
        - 音ゲーのscorepostはこちらでしています
    - [mstdn.cryptic-command.net](https://mstdn.cryptic-command.net/@s01)
        - 個人サーバーです。このサーバーのアカウントは技術出版サークルCryptic Commandのメンバー以外に払い出すつもりはありません
        - RubyコミュニティのCode of Conductだとグレーか黒い用法はこっちに倒れます。なので「フォロー申請は素性を知ってる人に限定しています」
            - 政治や国際情勢の話をします
            - 非R18含め二次元の話はこっちでします
            - その性質上、3ヶ月でpostの自動削除をします
- Keybase: [@s01](https://keybase.io/s01)
    - 何で私これ書いてなかったんだ
    - End-to-end暗号化がされているのでダイレクトメッセージを送る方法として強くおすすめです
    - [Keybaseって何？という方向けに記事を用意しています](/ja/how-to-keybase.html)
- SMTP: sylph01 at (gmailのドメイン名)
    - SMTPをやめろ。なので最終手段でお願いします

## おまけ: タイトルなど元ネタについて
### nightbird lost wing (メインタイトル)

nightbird lost wingはDanceDanceRevolution (2013)で登場した猫叉Master+の楽曲。複数のBEMANI機種に収録されている"Far east nightbird"の続編。タイトルもさることながら、曲中のカウントダウンが墜落への時間を数えているように聞こえる様がとても現状と重なってしまう。

### True Blue

True Blueはjubeat saucer初出のdj TAKA作曲・AiMEE歌唱の楽曲。"Blue Rain", "Broken"の流れをくんだ楽曲で、破れた恋から立ち直っていく心を歌った曲。dj TAKAの2ndアルバムは同楽曲からタイトルを取っている。


ぶっこ抜き音源の動画貼るのはあまりファンではないので本当は自身がプレイしてる動画貼りたいんだけど、腕を骨折してる[^9]のでDDRができなくてな…。

----

[^1]: 参考までに、Facebookは基本的に「リアルの人間関係」（学校、職場、オーケストラ）の人のみ友人登録しているが、本記事執筆時点で593人。
[^1p5]: いちおうこの記事の執筆時点で「非認証ユーザーで1000件まで回復した」「制限がなくなった」との報もあるが
[^2]: 真に選ばれた人しか残ってないわけではなく、情勢を見て自発的に抜けたトップ層がいるかもしれないというのは知ってるけど、そもそも一度エンジニアとして採用されてる時点でかなりの選抜を通ってるのは事実
[^3]: ここでの認証はverificationのこと
[^4]: 日本人女性名+乱数のスクリーンネームでbio欄に地名と「出会いを求めている」（穏当な表現）みたいな記述のあるアカウントに出会いの候補としてリストに突っ込まれたりするが、私がそんなにモテるわけがなかろう？
[^5]: コミュニティや趣味の"real-ness"については[過去にスマブラプレイヤーの不祥事とコミュニティの安全性について書いた文章](https://d.s01.ninja/entry/20200707/1594130400)で語っている
[^5p5]: mixiの仕様変更は囲い込みやマネタイズのための変更という性質は薄いように思うので「改悪」ではあるが「enshittification」とは違うかも、という気はする
[^6]: 私は実名をインターネットで伏せることは諦めているが、「実名を主として利用しているところでのインターネットでの別名義の開示、あるいはその逆」は攻撃とみなす、という運用をしている
[^7]: "We never knew love's a garden and hard to grow" (True Blueの歌詞), huh
[^9]: ホームのプレイヤーはnightbird lost wingのEXPERT譜面を指して、足の入る順序を間違えやすいので「骨折譜面」って言ってましたね…。