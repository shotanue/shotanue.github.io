---
title: "ことはじめ"
date: 2019-09-11T23:54:49+09:00
draft: false
toc: false

---

あまりにgithub pagesが貧相だったので、hugoを導入してみた。

hugoにした理由は、

- 簡単そう。
- どうも早いらしい。（記事をたくさん書かないとあまりメリットはなさそうではある）

みたいなところで選んだ。  
(Gatsbyもあるけど、ちょっと高級過ぎな気がしたのでやめた。もう少しWebアプリっぽいものを作るには良さそうな気はしているので、いつか触ってみたいとは思っている。)


ただ、hugoを導入したことで、Qiitaの技術記事とのバッティングはちょっと気になっている。まとまった文章垂れ流す上で「いいね」だったり「コメント」を気にしたくないのであればこっちのが良いと思うけど、
仮に技術記事書いて間違った知識を載せてしまった場合に指摘が入らなくなってしまうので。  

Qiita書いて、個人のブログも書いててみたいな人がいたとして、どう出し分けしているのか。


## 導入にあたって
hugoをチュートリアルで提示されているテーマをを無視して、いきなり好きなテーマを使ったらハマった。  
普通に[チュートリアル](https://gohugo.io/getting-started/quick-start/)を進めればよかったのだけども、指定されたテーマを無視して`hermit`を入れると、ローカルサーバーを立ち上げた時点でこんなエラーが出る。

```
Building sites … ERROR 2019/09/11 23:59:58 render of "taxonomy" failed: execute of template failed: template: _default/list.html:21:50: executing "main" at <.Site.Params.dateformshort>: invalid value; expected string
```

エラーメッセージ見てもよくわからなかったのでググってみると、同じテーマで同じ事象に遭遇している人がいたようで、[この設定が足りないよ](https://github.com/Track3/hermit/blob/3f7461d95d107895b359fbe910aebde666ed01fc/exampleSite/config.toml#L28-L32)って[回答](https://discourse.gohugo.io/t/quick-start-error/17310)があった。

テーマによっては変数みたいなのを設定ファイル(config.toml)で追加指定必須らしい。
（なんとなくテンプレート側で参照してる箇所を消せばそれはそれで解消できそうだけど）


ということで無事hugoが導入できた。


あと他では、Netlifyとかにホストできるらしいけど、ドメイン取ったりがめんどくさい、お金かかる、ので当分github pagesで運用してみる。

