---
layout: page
title: Tech Talks in the Age of LLMs - On LLMs and Us, Part 1.5
date: 2026-01-14 01:50:00 +09:00
category: ja
---

[BuriKaigi 2026](https://burikaigi.dev/)に参加してきて、その中で「[そこで聞ける技術の話なんてAIがいくらでも教えてくれる](https://x.com/junpai_code/status/2010127734165647495)」、というコメントを見かけて[^1]、思ったことがあったので書きます。

[^1]: この部分だけの抜き出しはかなり悪意があるという主張はわかるが、この主張に合意する部分もありつつ、そうではない部分について思うことがあった、というので書いているので、このツイートに対して部分的に抜き出して「何もわかってない」などと主張するものではありません

----

<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script> 

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">いうけどAIが教えてくれる程度のトークは通らないと思っており… あとAIはまだwhyの部分を表面的にしか教えてくれない</p>&mdash; sylph01 / G4きゅーぶ (@s01) <a href="https://twitter.com/s01/status/2010169461861667221?ref_src=twsrc%5Etfw">January 11, 2026</a></blockquote>

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">あと大事なことですがLLMは学習データの都合上現状追認しかしないので「SMTPをやめろ」とか言わない。思想を脱臭された意見しか吐けない（かAIサービス運営者に悪意がある場合は思想を食わされる）。なのでちゃんと人間が介入してるトークはこの時代かえって貴重なんですよ <a href="https://t.co/krLUTt6LXc">https://t.co/krLUTt6LXc</a></p>&mdash; sylph01 / G4きゅーぶ (@s01) <a href="https://twitter.com/s01/status/2010219557202604113?ref_src=twsrc%5Etfw">January 11, 2026</a></blockquote>

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">だから「スライド全部AIに生成させたったwwww」みたいに言うてる驚き屋の主張は「こういう時代だからこそ」必要ないんですよ。人のできることをやってくれ</p>&mdash; sylph01 / G4きゅーぶ (@s01) <a href="https://twitter.com/s01/status/2010220054424797234?ref_src=twsrc%5Etfw">January 11, 2026</a></blockquote>

----

LLMが環境に存在する時代のテックトークでは、（少なくとも経験者は）LLMが教えてくれる程度の内容のトークをすることは求められなくなったといえるし、それが暗黙の評価基準に入っていると言っても過言ではない、と考える。その上で私はそうでないトークをしているつもりである。（一方でそれなので自動的に価値が高い、という主張をしているのではない。価値は別のところにも現れてその部分でも評価される）

確かに事実のみのトークは「AIが教えてくれる」。一方でその事実に辿り着く過程はどうだろうか？例えば自分が出くわしたバグや障害対応などの経験から出てきている話であればその過程についてはAIは教えてくれない。経験そのものはn=1程度かもしれないが、それが十分小さいnであるためにまだ言語化されていない場合はそれはLLMは学習しておらず語る能力を持たない。十分小さいnでも他の人がハマりうることであればそれ自体は価値を持つ。その道筋が正確にプロンプトとして与えられれば学習した内容から事実を導出して語ることはできるかもしれないが、その道筋のプロンプト自体をLLMが思いつくことはできない。そこで基調講演でt-wadaさんが語っていた「AIは知識は持つが知恵を持たない」「AIは鍵のかかった引き出しの形で知識を持っている」「鍵を与えてあげる必要がある」という主張に繋がる。ハマった経験、障害対応の経験はLLMの引き出しの鍵かもしれない。その鍵を伴ったトークならば単純にLLMに出力させることはできず、鍵の存在がLLMの時代において価値であるといえる。

思想についても同様のことが言える。LLMは「SMTPをやめろ」について説明することはできるかもしれないが「SMTPをやめろ」と言い出すことはできない。「SMTPをやめろ」という主張に辿り着くには、LLMが出力できるような知識はもちろん、経験とそれに基づく意思が必要である。LLMは意思を補強することはできるかもしれないが自分自身が意思を持つことは（少なくとも現時点では）できない。またAIサービスの運営者の側で何らかの思想の制限をかけていることは十分にありうるので、そのような思想は当然AIサービスには出力できない[^2]。

[^2]: コミュニティガイドラインに反するような出力はLLMにはできない可能性があるが、だからといってコミュニティガイドラインに反する出力をすればいいというものではないが…

LLMの時代のテックトークは、思想のみを語るsoft talkであってもならず、既存の知識のみを語るsoft academism[^3]であってもならない。学問的新規性を伴うという厳密な意味でのhard academismである必要はなくとも、少なくとも知識のみを話すのではLLMができるため、経験や意思を伴う発表をする必要があるといえるだろう。

[^3]: 元は高山博『ハード・アカデミズムの時代』において提唱されている概念で、既存の知を再紹介するソフト・アカデミズムに対して、知を生み出す活動であるハード・アカデミズムを指向せよ、ということが主張されている。テックトークにおいてもソフト・アカデミズムを超克しハード・アカデミズムを指向するべきである、ということについては[RubyKaigi 2024の感情処理記事](https://d.s01.ninja/entry/20240521/1716275951)や[Kaigi on Rails 2021の登壇記録](https://d.s01.ninja/entry/20211025/1635159600)に書いた。

人間のやれることをやっていきましょう。「AIが全部教えてくれるからカンファレンスは無意味」みたいな主張をひっくり返せるようなトークをしていきましょう。それがAIの学習を強化するとしても、まあそれはそれとしていいじゃないですか。

----

私のトークについて、またBuriKaigiそのものの体験については別途記事を書くかもしれません。[スライドはこちら](https://speakerdeck.com/sylph01/ren-ming-wojiu-uji-shu-tositenoend-to-endan-hao-hua-tomessaging-layer-security)。だいたい過去2回の反省を経てだいぶ説明を改善したつもりだがやっぱり30分でやるのは極めて無理がある。Part 1、特にp11〜22の部分が「LLMには出力できない部分」です。テクニカルな部分についてはだいたい以下の部分がわかると気持ちがわかると思います:

- [p41: MLSの構成要素3つで最終的にやりたいこと](https://speakerdeck.com/sylph01/ren-ming-wojiu-uji-shu-tositenoend-to-endan-hao-hua-tomessaging-layer-security?slide=41)
- [p42: Secret Tree](https://speakerdeck.com/sylph01/ren-ming-wojiu-uji-shu-tositenoend-to-endan-hao-hua-tomessaging-layer-security?slide=42)
- [p46: Key Schedule](https://speakerdeck.com/sylph01/ren-ming-wojiu-uji-shu-tositenoend-to-endan-hao-hua-tomessaging-layer-security?slide=46)
- [p59: TreeKEMの部分はわからなくてすっ飛ばしても動作はするけど計算量が変わるよっていう説明](https://speakerdeck.com/sylph01/ren-ming-wojiu-uji-shu-tositenoend-to-endan-hao-hua-tomessaging-layer-security?slide=59)
  - グループ人数が1000人ってことはありうる。例えばDiscordサーバーのチャットにE2EE入れたいってなったら1000人や10万人のグループっていうのはありうる

----

Part 2はそのうち書きます。ネタ本はまあ読み終えた。

----
