---
layout: page
title: Guide to WiFi on PicoRuby/R2P2
date: 2024-05-04 14:00:00 +09:00
category: en
---

This is a preview of what's to come in my RubyKaigi 2024 talk, and also a guide for people who want to try what I will be talking/have talked at RubyKaigi 2024.

Links to relevant repositories are below:

- PicoRuby
  - [Official](https://github.com/picoruby/picoruby)
  - [Fork](https://github.com/sylph01/picoruby) (has the `wlan` branch)
- R2P2
  - [Official](https://github.com/picoruby/R2P2)
  - [Fork](https://github.com/sylph01/R2P2) (has the `wlan` branch)

Please note that I am omitting explanations of what PicoRuby and R2P2 are. Please go check the official repository for those.

## Update 8/27/2024

WiFi in PicoRuby/R2P2 has been merged, so [I wrote a quick update document about how to build with WiFi(in Japanese)](/ja/picoruby-wifi-build.html). Also, [I wrote an article on my RubyKaigi talk in Rubyist Magazine vol.0064 (also in Japanese)](https://magazine.rubyist.net/articles/0064/AddingSecurityToMicrocontrollerRubyJa.html).

(日本語) マージされたので[ビルド方法について速報記事を書きました](/ja/picoruby-wifi-build.html)。また、[Rubyist Magazine 0064号にRubyKaigiのトークの日本語解説記事があります](https://magazine.rubyist.net/articles/0064/AddingSecurityToMicrocontrollerRubyJa.html)。

## Building PC version R2P2

You can build R2P2 that runs on PC with `MRUBY_CONFIG=r2p2-bin` option. This is used to check functionality that doesn't depend on devices. I used this to test the cryptography implementation.

The following will get you from a full-scratch Ubuntu installation to building the PC version R2P2. Your mileage may vary but you will most likely start after installing the latest Ruby version.

```shell
# build latest ruby
sudo apt install git build-essential autoconf patch rustc libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libgmp-dev libncurses5-dev libffi-dev libgdbm6 libgdbm-dev libdb-dev uuid-dev
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
echo 'eval "$(~/.rbenv/bin/rbenv init - bash)"' >> ~/.bashrc
git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
rbenv install -l
rbenv install 3.3.1
rbenv global 3.3.1

# install gcc-arm-none-eabi
sudo apt install gcc-arm-none-eabi

# clone PicoRuby, then build PC version R2P2
git clone --recursive https://github.com/picoruby/picoruby.git
MRUBY_CONFIG=r2p2-bin rake

# run PC version R2P2
build/no-libc-host/bin/r2p2
```

## Building R2P2

### Preparing the build environment

- CMake from `apt` was too old to build R2P2, so I had to build CMake from source.
  - [Download archive](https://cmake.org/download/) and untar it
  - `./bootstrap`, `make`, `sudo make install`
- Clone R2P2, then `git submodule init`, `git submodule update`
- Clone `pico-sdk`, `pico-extras`, then specify their paths into environment variables `PICO_SDK_PATH` and `PICO_EXTRAS_PATH`
  - [pico-sdk](https://github.com/raspberrypi/pico-sdk)
  - [pico-extras](https://github.com/raspberrypi/pico-extras)
- Inside pico-sdk, `git submodule init` and `git submodule update`
- Inside pico-sdk, `git checkout 1.5.1`
- Inside pico-extras, `git checkout sdk-1.5.1`
  - Note that the tag name is a little different
- Inside R2P2 you will be able to `rake` now

### Troubleshooting

- Something around the line of `PICO_SDK_FETCH_FROM_GIT` and/or `PICO_EXTRAS_FETCH_FROM_GIT`: Forget it, set `PICO_SDK_PATH` and `PICO_EXTRAS_PATH` explicitly. As far as I know the automatic setup won't automatically set your pico-sdk and pico-extras version to the one that R2P2 requires, so it's better to just do them manually.
- `(PATH) does not contain a CMakeLists.txt file` on `pico_extras_import.cmake:62`: There is build cache from previous builds that is interfereing with the current build. Delete the content of `build/` directory.
- `#include <bsp/board.h>` giving `bsp/board.h: No such file or directory`: `pico-sdk`'s submodules aren't updated.

## Connect to R2P2

### Load `.uf2` file

Connect your Pico W while pressing the BOOTSEL button. This will give you a storage device named `RPI-RP2`. Drag and drop the `.uf2` file that contains the R2P2 executable (either the one downloaded from GitHub releases or the one you've built). After the file has been copied, a storage device named `R2P2` will come up on your system. This means that you're ready to connect with the following steps.

### On a Linux

- Connect your Pico W to your computer
- `gtkterm --port /dev/ttyACM0`

### On a Mac

- Connect your Pico W to your computer
- After the storage "R2P2" appears on your desktop:
  - `screen /dev/tty.usbmodem1234567890121 115200`
    - First argument is the console port
    - Second argument is the baud rate. Sometimes works without this but I get more stable results with this

## Blink the on-board LED

...because that's like Hello World in IoT!

The code written in the rest of the article will be run under `irb`. There is an option to run code by putting `app.rb`/`app.mrb` inside the Pico W's storage but here we're doing it on irb.

```ruby
CYW43.init "JP"
pin = CYW43::GPIO.new(CYW43::GPIO::LED_PIN)
pin.write(1)
pin.write(0)
```

Note that in a Pico without the WiFi chip the LED pin is not under the CYW43439 chip so it runs like:

```ruby
led = GPIO.new(25, GPIO::OUT)
led.write(1)
led.write(0)
```

## Connect to WiFi

To try this out, you will have to [checkout sylph01/R2P2 `wlan` branch](https://github.com/sylph01/R2P2/tree/wlan). The submodule points to a commit under [sylph01/picoruby's `wlan` branch](https://github.com/sylph01/picoruby/tree/wlan), so after checking out `wlan` branch, go inside `lib/picoruby`, set remote to `sylph01/picoruby`, then check out the `wlan` branch.

After R2P2 and `lib/picoruby` points to the `wlan` branch, build R2P2 with `BOARD=pico_w rake`. If this is your first time building and cloning Mbed TLS, some Mbed TLS functions might cause an error saying they lack references or something. Retry the same command again, and the build will complete. After the build completes, you will get the `.uf2` file under `build/` directory, so copy that onto your Pico W.

Here are some stuff you can try out:

```ruby
# Connect to WiFi
CYW43.init('JP')
CYW43.enable_sta_mode
CYW43.connect_blocking('your_aps_ssid', 'password', CYW43::Auth::WPA2_MIXED_PSK)

# Resolve DNS names
require 'net'
Net::DNS.resolve('google.com')
Net::DNS.resolve('s01.ninja')
Net::DNS.resolve('192.168.11.1')
Net::DNS.resolve('nonexistent-domain.org')

# HTTPS GET
require 'net'
client = Net::HTTPSClient.new('example.com')
r = client.get('/')
```