---
title: "PHP MySQL 8.0 Auth"
date: 2019-11-04T22:46:06+09:00
draft: false
toc: false
images:
tags: 
  - php
  - docker
  - mysql
---

## ことはじめ

mysql:8.0にpdoでつなぎに行ったらこんな感じのエラーが出た。

```bash
PDOException: PDO::__construct(): The server requested authentication method unknown to the client [caching_sha2_password] in /var/app/bin/setup.php on line 19
```

認証方法が8系から変わった？とのことなので、調べる。

## 調べた

[この記事](https://symfoware.blog.fc2.com/blog-entry-2160.html)によると、my.confあたりに

```bash
[mysqld]
# 略
default-authentication-plugin=mysql_native_password
```

を追加すると良いとのこと。

## 解決

mysqlの公式dockerイメージで構築していたので、したのような感じでCMDを渡してあげる。


```bash
version: "3.7"

services:
  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: root
    command:
      - --character-set-server=utf8mb4
      - --collation-server=utf8mb4_unicode_ci
      - --default-authentication-plugin=mysql_native_password

```

#### 補足
- mysqldの設定をコンテナ起動時にCMDから指定の形式で渡すと変更ができる
- 渡せるコマンドは今回の場合だと`docker-compose run db --verbose --help`で確認ができる



> Configuration without a cnf file
> Many configuration options can be passed as flags to mysqld. This will give you the flexibility to customize the container without needing a cnf file. For example, if you want to change the default encoding and collation for all tables to use UTF-8 (utf8mb4) just run the following:
>

```bash
 $ docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
```
>If you would like to see a complete list of available options, just run:

```bash 
$ docker run -it --rm mysql:tag --verbose --help
```



## ref
- https://symfoware.blog.fc2.com/blog-entry-2160.html

- https://hub.docker.com/_/mysql


