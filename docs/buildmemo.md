# ビルド関連メモ

## bungeecordの自動テストが通らない件

`docker-compose build` する際にbungeecordコンテナで `mvn clean install` して必要なモジュールを
mvnから取ってきて自動テストまでしてくれるが、google.Jsonみたいなモジュールの自動テストが通らない。

> JsonのParse Error らしいが、テスト用のjsonデータが悪いんじゃ？？？

埒があかないので、テストをスキップする。他の人はどうしているんだろう。

```diff
RUN git checkout 934e4543d695f1e79f6690ac4ee435e3fa5d50d4
- RUN mvn clean install
+ RUN mvn clean install -DskipTests=true
```

## コードの修正が終わったら

sbtシェルにて、以下のコマンドを実行してフォーマットをしておかないと検証ではねられます。
```scala
assembly; scalafmtAll; scalafixAll;
```

## コンベンショナルコミットのtype情報

https://gist.github.com/brianclements/841ea7bffdb01346392c#type

- build: Changes that affect the build system or external dependencies (example scopes: gulp, broccoli, npm)
- ci: Changes to our CI configuration files and scripts (example scopes: Circle, BrowserStack, SauceLabs)
- docs: Documentation only changes
- feat: A new feature
- fix: A bug fix
- perf: A code change that improves performance
- refactor: A code change that neither fixes a bug nor adds a feature
- style: Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)
- test: Adding missing tests or correcting existing tests


## テスト用のワールドを作成する方法

ゲーム内からmultiverseのコマンドを実行すれば良いはずなのだが。
コマンドの実行パーミッションがないと怒られる。

### パーミッション付与方法は？

調査中

### 作成するワールド名

com/github/unchama/seichiassist/ManagedWorld.scala
```scala
  case object WORLD_2 extends ManagedWorld("world_2", "メインワールド")
  case object WORLD_SW extends ManagedWorld("world_SW", "第一整地ワールド")
  case object WORLD_SW_2 extends ManagedWorld("world_SW_2", "第二整地ワールド")
  case object WORLD_SW_3 extends ManagedWorld("world_SW_3", "第三整地ワールド")
  case object WORLD_SW_4 extends ManagedWorld("world_SW_4", "第四整地ワールド")
  case object WORLD_SW_NETHER extends ManagedWorld("world_SW_nether", "整地ネザー")
  case object WORLD_SW_END extends ManagedWorld("world_SW_the_end", "整地エンド")
```
これだけ押さえておけばよいだろう。

しかしM1 MacのMinecraftクライアントはアンダーバーが入力できんのだよ...

参考：
https://wiki.craftportal.jp/view/%E3%83%97%E3%83%A9%E3%82%B0%E3%82%A4%E3%83%B3%E8%A7%A3%E8%AA%AC:Multiverse-Core

