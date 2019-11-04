---
title: "Alpine Composer"
date: 2019-11-04T19:10:38+09:00
draft: false
toc: false
images:
tags: 
  - php
  - composer
  - docker
---

コンポーザーをalpineに楽に入れたい。
(※alpine以外の環境にこの方法で入れて動くかは不明)

結論としては、こんな感じにすればOK

```bash
FROM alpine:3.10.3

ENV COMPOSER_HOME /composer
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV PATH /composer/vendor/bin:$PATH

COPY --from=composer:1.9.1 /usr/bin/composer /usr/bin/composer
```

## 解説
composerは公式イメージがあるので、ここから実行ファイルだけ落としてくれば動くみたい。  
/usr/binとかに入れておく。  

#### `COMPOSER_ALLOW_SUPERUSER 1`
これを指定すると、composerをSUPER USERで動かしても怒らなくなる。  
alpineはそのままだとSUPER USERで動くので指定している。  
実行ユーザーを設定していたりすれば不要なんだろうと思う。  

#### `COMPOSER_HOME`
指定したディレクトリにcomposerそのものが動作するのに使用するファイル、キャッシュ類が格納される。


## 応用
ユースケースとして、デプロイ用のイメージでcomposer installをしたいが、アプリ動作時はcomposerが不要になる場合は下記のようにするとイメージが綺麗に保てるのでは(と思っている)。

```bash
FROM php:latest

COPY --from=composer:1.9.1 /usr/bin/composer /usr/bin/composer

WORKDIR /var/some-application
COPY ./some-application .

RUN composer install --no-dev \
 && composer some-application-set-up-script \
 && rm -rf /usr/bin/composer /composer
```

