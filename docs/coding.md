# コーディング関連メモ

## cats（非同期処理ライブラリ）について

https://typelevel.org/cats-effect/

### クラス相関図@りとさん感謝
https://cdn.jsdelivr.net/gh/tpolecat/cats-infographic@master/cats.svg

## プルリクエスト後のレビューと修正について

- github上でsuggestをcommitする際は慎重に！
  - 長いコミットメッセージがデフォルトで入っているので、必ずコンベンショナルコミットに沿ったメッセージに変更すること
  - suggestをcommitした後はローカルのコードをpullして競合が起こらないよう配慮すること。

### コミットログを修正する方法

```sh
git log --oneline # これで戻したいコミットログが上から何行目にあるか調べて
git rebase -i HEAD~n # nは戻したいコミットが上から何番目にあるか
# エディタが開くので戻したいコミットのハッシュ値の先頭にかかれている"pick"を"r"に変更して保存終了
# すると変更対象のコミット上方が再びエディタで開かれるのでコミットメッセージを修正し、保存終了
git rebase --continue # continueできなくなるまで続ける
git push origin branch_name
```

ただしrebaseで戻したコミットでコンクリフトが発生する場合はpickを編集したあとコンクリフトエラーになってしまう。
この状態で競合解消すると新しいコミットが作られてしまうのでコミットメッセージの変更ができない。

### 無理やり枝を差し替える方法
@Koryさん提供方法。感謝。

- rebaseで最新コミットの状態に戻す
- 新しいブランチを作る（ブランチ名は何でもよい）
- 必要なコミットをチェリーピックで新ブランチに適用する
- （この間にどんな操作をすればよいか忘れた）
- 元の

## scalaFix, scalaFmt後の順番

多分scalaFixを先にしないとダメ。

## scalaFix, scalaFmt後の操作に注意

エディタ上で対象ファイルを開いたままscalaFix,scalaFmtすると「ディスク上のファイル」か「メモリ上のファイル」のどちらを表示するか聞かれる。
ここで「メモリ上のファイル」を選択（デフォルトがそっちになってる）をするとfmtする前のデータが残ってしまう。

fmtする前にエディタを閉じるか、きちんとディスク上のファイルを選択しなければならない。

## fmtしてもcommit出来なかった場合

IDEA上ではなくコマンドかVSCodeでGit操作しましょう。IDEAのGitクライアントはなんか変。

## PRマージ後、フォーク元のリポジトリから変更を自リポジトリに同期する方法

https://qiita.com/Nossa/items/ace2ab802adc85f86b20

```sh
# これは一度だけ
$ git remote add upstream https://github.com/GiganticMinecraft/SeichiAssist.git
# フォーク元の master ブランチの変更差分をフェッチます。
# ※upstream/master に保管されます
$ git fetch upstream

# masterブランチをチェックアウトしフォーク元の差分をマージします。
$ git checkout master
$ git merge upstream/master

# 最後に自分の修正ブランチに master ブランチを取り込みます。コンフリクトが出たら解決します。
$ git checkout 自分の修正ブランチ
$ git merge master
```