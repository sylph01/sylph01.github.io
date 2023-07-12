---
layout: page
title: Mastodonの自動アーカイブ
date: 2023-07-12 18:30:00 +09:00
category: ja
---

ただのtech memo。

## リンク

- [systemd/ユーザー - ArchWiki](https://wiki.archlinux.jp/index.php/Systemd/%E3%83%A6%E3%83%BC%E3%82%B6%E3%83%BC)
- [systemd/タイマー - ArchWiki](https://wiki.archlinux.jp/index.php/Systemd/%E3%82%BF%E3%82%A4%E3%83%9E%E3%83%BC)
- [mastodon-archive - PyPI](https://pypi.org/project/mastodon-archive/)

## 背景

@s01@mstdn.cryptic-command.net は3ヶ月で自動post削除をするようにしているが、アカシックレコードとしてTwitter-likeなものを使ってきたので同様の性質は欲しい。

## mastodon-archiveを入れる

pip3を動かせるようにするまでの手順は省略[^1]。

```sh
pip3 install --user mastodon-archive
```

### archive.sh

```sh
#!/bin/bash

today=$(date +"%Y%m%d")

mastodon-archive archive s01@mstdn.cryptic-command.net
mv mstdn.cryptic-command.net.user.s01.json "s01-$today.json"
```

## systemdのUser TimerとUser Serviceを設定する

### ~/.config/systemd/user/mstdn-archive.timer

```sh
[Unit]
Description=Archive mstdn.cryptic-command.net posts

[Timer]
OnCalendar=Sun 00:00

[Install]
WantedBy=timers.target
```

### ~/.config/systemd/user/mstdn-archive.service

```sh
[Unit]
Description=service for mstdn archive

[Service]
Environment="PATH=/home/ubuntu/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin"
Type=oneshot
WorkingDirectory=/home/ubuntu/mstdn-archive
ExecStart=/bin/bash /home/ubuntu/mstdn-archive/archive.sh
```

### その後

```sh
systemctl --user daemon-reload
systemctl --user enable mstdn-archive.timer
systemctl --user start mstdn-archive.timer
systemctl --user start mstdn-archive.service # run the contents of the timer manually
journalctl --user -xeu mstdn-archive.service # check log
```

[^1]: 私はﾋﾟｭﾄｰﾝをそんなに使わないので実はここもハマりどころではあるんだが…