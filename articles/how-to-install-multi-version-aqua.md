---
title: "aqua で良い感じにツールを複数バージョンインストールする方法"
emoji: "🤖"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["aqua"]
published: false
---

## 背景

仕事で同じツールを複数のリポジトリで使っていて、それぞれのバージョンが異なるケースはよくあるかと思います。
例えば、リポジトリ A では Go v1.24 を使っているけど、リポジトリ B では Go v1.20 を使っているみたいなケースです。

バージョン管理ツールとして aqua を使っている場合、
こういったケースはリポジトリのルートに aqua.yaml を配置するだけで済むのですが、
aqua.yaml は git の追跡に含めたくないという課題があります（ささやかな課題ですが）。

リポジトリの .gitignore に aqua.yaml を追加するのはリポジトリが OSS の場合は事実上無理ですし、
会社で使うリポジトリであってもやりたくはないので、何か良い方法はないかなと考えつつ放置してました。

自分なりに良い方法を考えたので、軽く記事にまとめておきます。

## やること

- リポジトリのルートに .local.aqua.yaml を配置
- global gitignore に .local.aqua.yaml を追加

以上です。めっちゃ簡単です（この程度悩むまでもなく一瞬で考えつきたかった）。

できねえ...


```
❯ FATA[0000] aqua failed                                   aqua_version=2.27.3 env=darwin/amd64 error="open /Users/sho/src/github.com/ishii1648/zenn-articles/.aqua-me.yaml: no such file or directory" exe_na ~/src/github.com/ishii1648/zenn-articles
```


configFilePath が明示的に指定されている場合は、ファイル探索をストップする実装になっている
https://github.com/aquaproj/aqua/blob/main/pkg/config-finder/config_finder.go#L87-L89

意図的に明示的にファイル探索ストップするようにした経緯があるので、AQUA_CONFIG に設定する方法は無理そう
https://github.com/aquaproj/aqua/issues/1410


探索ファイルをもう少しファジーにできれば `.local.aqua.yaml` のようなファイルを配置すればいけるが
https://aquaproj.github.io/docs/reference/config/#configuration-file-path

issue で聞いてみた
https://github.com/aquaproj/aqua/issues/3700
