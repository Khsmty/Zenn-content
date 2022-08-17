---
title: "Ubuntu や Debian に最新の Node.js をインストールする"
emoji: "🐣"
type: "tech"
topics: ["nodejs", "ubuntu", "debian"]
published: true
---

# はじめに

Ubuntu には、OS 標準のソフトウェアというものが存在します。
試しに、手元の Ubuntu 22.04 LTS での Node.js 標準バージョンをチェックしてみます。

```bash
$ apt info nodejs
Package: nodejs
Version: 12.22.9~dfsg-1ubuntu3
Priority: extra
Section: universe/web
Origin: Ubuntu
Maintainer: Ubuntu Developers <ubuntu-devel-discuss@lists.ubuntu.com>
Original-Maintainer: Debian Javascript Maintainers <pkg-javascript-devel@alioth-lists.debian.net>
Bugs: https://bugs.launchpad.net/ubuntu/+filebug
Installed-Size: 932 kB
Provides: node-types-node (= 12.20.42~12.22.9~dfsg-1ubuntu3)
Depends: libc6 (>= 2.34), libnode72 (= 12.22.9~dfsg-1ubuntu3)
Recommends: ca-certificates, nodejs-doc
Suggests: npm
Breaks: node-babel-runtime (<< 7), node-typescript-types (<< 20210110~)
Homepage: https://nodejs.org/
Download-Size: 122 kB
APT-Sources: http://jp.archive.ubuntu.com/ubuntu jammy/universe amd64 Packages
Description: evented I/O for V8 javascript - runtime executable
 Node.js is a platform built on Chrome's JavaScript runtime for easily
 building fast, scalable network applications. Node.js uses an
 event-driven, non-blocking I/O model that makes it lightweight and
 efficient, perfect for data-intensive real-time applications that run
 across distributed devices.
 .
 Node.js is bundled with several useful libraries to handle server
 tasks:
 .
 System, Events, Standard I/O, Modules, Timers, Child Processes, POSIX,
 HTTP, Multipart Parsing, TCP, DNS, Assert, Path, URL, Query Strings.
```

![](https://i.gyazo.com/3c92877d67bf24bd2f3e852906e5544c.png)

Node.js 12.22.9 のリリース日は 2022 年 01 月 10 日と、そこまで古すぎるわけではないものの、現在の最新 LTS は 16.x 系で、12.x 系のサポートを終了するパッケージも多くなってきました。

そこで、本記事では、Ubuntu や Debian に最新の Node.js をインストールする方法を紹介します。

# 最新パッケージの取得

Node.js では、面倒なリポジトリの追加や公開鍵のインポートなどの作業を自動でやってくれるスクリプトが提供されています。
また、このスクリプトは Node.js 公認の企業である NodeSource が提供しているため、安心して利用できます。

それでは、実際にそのスクリプトを使って最新の Node.js をインストールしてみます。

LTS 版 (v16.x) か最新版 (v18.x)、または特定のバージョンを指定してインストールします。
(詳しくは [こちら](https://github.com/nodesource/distributions#installation-instructions) を参照してください)
ここでは、LTS 版と最新版のコマンドを掲載します。

:::details Node.js LTS (16.x)
```bash
# Ubuntu の場合
$ curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
$ sudo apt-get install -y nodejs

# Debian で root を使う場合
$ curl -fsSL https://deb.nodesource.com/setup_lts.x | bash -
$ apt-get install -y nodejs
```
:::

:::details Node.js 最新版 (18.x)
```bash
# Ubuntu の場合
curl -fsSL https://deb.nodesource.com/setup_current.x | sudo -E bash -
sudo apt-get install -y nodejs

# Debian で root を使う場合
curl -fsSL https://deb.nodesource.com/setup_current.x | bash -
apt-get install -y nodejs
```
:::

実際にコマンドを実行した結果が以下です。
今回は、LTS 版をインストールしました。

```bash
$ curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -

## Installing the NodeSource Node.js 16.x repo...

## Populating apt-get cache...

+ apt-get update

(省略)

## Confirming "jammy" is supported...

+ curl -sLf -o /dev/null 'https://deb.nodesource.com/node_16.x/dists/jammy/Release'

## Adding the NodeSource signing key to your keyring...

+ curl -s https://deb.nodesource.com/gpgkey/nodesource.gpg.key | gpg --dearmor | tee /usr/share/keyrings/nodesource.gpg >/dev/null

## Creating apt sources list file for the NodeSource Node.js 16.x repo...

+ echo 'deb [signed-by=/usr/share/keyrings/nodesource.gpg] https://deb.nodesource.com/node_16.x jammy main' > /etc/apt/sources.list.d/nodesource.list
+ echo 'deb-src [signed-by=/usr/share/keyrings/nodesource.gpg] https://deb.nodesource.com/node_16.x jammy main' >> /etc/apt/sources.list.d/nodesource.list

## Running `apt-get update` for you...

+ apt-get update

(省略)

## Run `sudo apt-get install -y nodejs` to install Node.js 16.x and npm
## You may also need development tools to build native addons:
     sudo apt-get install gcc g++ make
## To install the Yarn package manager, run:
     curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | gpg --dearmor | sudo tee /usr/share/keyrings/yarnkey.gpg >/dev/null
     echo "deb [signed-by=/usr/share/keyrings/yarnkey.gpg] https://dl.yarnpkg.com/debian stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
     sudo apt-get update && sudo apt-get install yarn
```

では、最新版の Node.js が取得できているか確認しましょう。

```bash
$ apt show nodejs
Package: nodejs
Version: 16.17.0-deb-1nodesource1
Priority: optional
Section: web
Maintainer: Ivan Iguaran <ivan@nodesource.com>
Installed-Size: 128 MB
Provides: nodejs-dev, nodejs-doc, nodejs-legacy, npm
Depends: libc6 (>= 2.17), libgcc1 (>= 1:3.4), libstdc++6 (>= 4.8), python3-minimal, ca-certificates
Conflicts: nodejs-dev, nodejs-doc, nodejs-legacy, npm
Replaces: nodejs-dev (<= 0.8.22), nodejs-legacy, npm (<= 1.2.14)
Homepage: https://nodejs.org
Download-Size: 27.1 MB
APT-Manual-Installed: yes
APT-Sources: https://deb.nodesource.com/node_16.x jammy/main amd64 Packages
Description: Node.js event-based server-side javascript engine
 Node.js is similar in design to and influenced by systems like
 Ruby's Event Machine or Python's Twisted.
 .
 It takes the event model a bit further - it presents the event
 loop as a language construct instead of as a library.
 .
 Node.js is bundled with several useful libraries to handle server tasks :
 System, Events, Standard I/O, Modules, Timers, Child Processes, POSIX,
 HTTP, Multipart Parsing, TCP, DNS, Assert, Path, URL, Query Strings.
```

先ほどは 12.22.9 だった Version が 16.17.0 に変わっており、正常に取得できていることが分かりました。

# Node.js のインストール

インストール自体はすごく簡単で、以下のコマンドを実行するだけです。

```bash
$ sudo apt install -y nodejs
```

インストールが終わったら、晴れて最新版の Node.js と npm を使用できるようになります。

```bash
$ node --version
v16.17.0

$ npm --version
8.15.0
```

![](https://i.gyazo.com/722ec65dc39b06d323df7d3af5db9704.png)

最新版ですね。

# まとめ

以上、Ubuntu へ最新版の Node.js をインストールする方法について紹介しました。
この記事が少しでも役に立ったら幸いです。
ここまで読んでいただきありがとうございました。

## 参考

- [nodesource/distributions: NodeSource Node.js Binary Distributions](https://github.com/nodesource/distributions)
