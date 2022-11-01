---
layout: page
title: Building Old Erlang Versions with Old OpenSSL
date: 2022-11-01 09:00:00 +09:00
category: en
---

(This is more of a language-independent tech memo)

- Go to https://www.openssl.org/source/ and download the tar for OpenSSL 1.1.1 (here we're using 1.1.1q)
- `tar xvzf openssl-1.1.1q.tar.gz`
- `cd openssl-1.1.1q/`
- `./config --prefix=/opt/openssl-1.1.1`
- `make`
- `make test`
- `sudo make install`
- `sudo mv /opt/openssl-1.1.1/ssl/certs /opt/openssl-1.1.1/ssl/certs.installed`
- `sudo ln -s /etc/ssl/certs /opt/openssl-1.1.1/ssl/certs`
- `export KERL_CONFIGURE_OPTIONS="--disable-debug --disable-silent-rules --without-javac --enable-shared-zlib --enable-dynamic-ssl-lib --enable-hipe --enable-sctp --enable-smp-support --enable-threads --enable-kernel-poll --with-ssl=/opt/openssl-1.1.1"`
- `asdf install erlang 22.3.4.21`