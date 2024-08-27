---
layout: page
title: 【速報】PicoRuby/R2P2のWiFiビルド
date: 2024-08-27 13:00:00 +09:00
category: ja
---

RubyKaigi 2024で発表したPicoRuby/R2P2のWiFi対応がmasterにmergeされました。

<blockquote class="twitter-tweet"><p lang="qme" dir="ltr"><a href="https://t.co/o5I3FIW9Of">https://t.co/o5I3FIW9Of</a> 🎉🎉🎉🎉🎉🎉</p>&mdash; sylph01 (@s01) <a href="https://twitter.com/s01/status/1828051249574633863?ref_src=twsrc%5Etfw">August 26, 2024</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

なのでこの記事では速報としてR2P2のWiFi対応ビルドを得る方法を記述します。公式のドキュメントが更新されるまでの速報版としてご利用ください。

- [PicoRuby](https://github.com/picoruby/picoruby)
- [R2P2](https://github.com/picoruby/R2P2)

なお以下環境はUbuntu(手元の環境は24.04)を前提とします。Macの場合は依存関係のインストールがちょっと違います（がこの部分は公式レポジトリのドキュメントに記事がある）。

【宣伝】[Rubyist Magazine 0064号にRubyKaigiのトークの日本語解説記事があります](https://magazine.rubyist.net/articles/0064/AddingSecurityToMicrocontrollerRubyJa.html)。よろしくねよろしくね

## 事前準備

だいたい5/4の英語記事を訳しただけ。

- `apt` から得られるCMakeは古いのでソースビルドする
  - [アーカイブをダウンロード](https://cmake.org/download/)してuntar
  - `./bootstrap`, `make`, `sudo make install`
- R2P2をクローン、 `git submodule init`, `git submodule update`
- `pico-sdk`, `pico-extras` をクローン、それぞれのパスを `PICO_SDK_PATH` と `PICO_EXTRAS_PATH` に設定
  - [pico-sdk](https://github.com/raspberrypi/pico-sdk)
  - [pico-extras](https://github.com/raspberrypi/pico-extras)
- `pico-sdk` 内にて、 `git submodule init` → `git submodule update`
- `pico-sdk` 内にて、 `git checkout 1.5.1`
- `pico-extras` 内にて、 `git checkout sdk-1.5.1`
  - タグ名がpico-sdkと違うことに注意

ここまでやるとR2P2の `rake` ができる。

## WiFi機能merge後のPico W向けのビルド方法

- WiFiあり、BLEなし `BOARD=pico_w WIFI=yes rake`
- WiFiなし、BLEあり `BOARD=pico_w BLE=yes rake`
- WiFiあり、BLEあり `BOARD=pico_w WIFI=yes BLE=yes rake`
- どちらもなしのビルドはできない

## メモ

1回目のビルドの際に以下のようなエラーを得たらもう一度同じコマンドでビルドすれば通る。

```
/usr/lib/gcc/arm-none-eabi/13.2.1/../../../arm-none-eabi/bin/ld: /home/sylph01/projects/b/R2P2/lib/picoruby/build/r2p2_w_wifi-cortex-m0plus/lib/libmruby.a(cmac.o): in function `c_digest':
/home/sylph01/projects/b/R2P2/lib/picoruby/mrbgems/picoruby-mbedtls/src/cmac.c:66:(.text.c_digest+0x10): undefined reference to `mbedtls_cipher_cmac_finish'
/usr/lib/gcc/arm-none-eabi/13.2.1/../../../arm-none-eabi/bin/ld: /home/sylph01/projects/b/R2P2/lib/picoruby/build/r2p2_w_wifi-cortex-m0plus/lib/libmruby.a(cmac.o): in function `c_reset':
/home/sylph01/projects/b/R2P2/lib/picoruby/mrbgems/picoruby-mbedtls/src/cmac.c:52:(.text.c_reset+0xc): undefined reference to `mbedtls_cipher_cmac_reset'
/usr/lib/gcc/arm-none-eabi/13.2.1/../../../arm-none-eabi/bin/ld: /home/sylph01/projects/b/R2P2/lib/picoruby/build/r2p2_w_wifi-cortex-m0plus/lib/libmruby.a(cmac.o): in function `c_update':
/home/sylph01/projects/b/R2P2/lib/picoruby/mrbgems/picoruby-mbedtls/src/cmac.c:40:(.text.c_update+0x42): undefined reference to `mbedtls_cipher_cmac_update'
/usr/lib/gcc/arm-none-eabi/13.2.1/../../../arm-none-eabi/bin/ld: /home/sylph01/projects/b/R2P2/lib/picoruby/build/r2p2_w_wifi-cortex-m0plus/lib/libmruby.a(cmac.o): in function `c__init_aes':
/home/sylph01/projects/b/R2P2/lib/picoruby/mrbgems/picoruby-mbedtls/src/cmac.c:18:(.text.c__init_aes+0x46): undefined reference to `mbedtls_cipher_cmac_starts'
collect2: error: ld returned 1 exit status
gmake[2]: *** [CMakeFiles/R2P2_W_WIFI-FLASH_MSC-0.2.1-20240827-8744982.dir/build.make:4823: R2P2_W_WIFI-FLASH_MSC-0.2.1-20240827-8744982.elf] エラー 1
gmake[1]: *** [CMakeFiles/Makefile2:1850: CMakeFiles/R2P2_W_WIFI-FLASH_MSC-0.2.1-20240827-8744982.dir/all] エラー 2
gmake: *** [Makefile:91: all] エラー 2
rake aborted!
Command failed with status (2): [cmake --build build]
/home/sylph01/projects/b/R2P2/Rakefile:114:in `block in <top (required)>'
Tasks: TOP => default => all => build
(See full trace by running task with --trace)
```

たぶん直せそうな気はするので見てみようとは思いますがビルドできない状態をとりあえず回避する方法は上記です。