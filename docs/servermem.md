# サーバー設定関連メモ

## 流体を流れないようにする

operator権限を持つプレイヤーがやったほうが速い。

```
# まず領域を作成しなければならない
//pos1 -1000,0,-1000 # 左上目いっぱい座標
//pos2 1000,255,0000 # 右上目いっぱい座標
/region define world # <- "world"という名前の領域を定義
/region flag world water-flow deny
/region flag world lava-flow deny
```

サーバーにattachして作業する場合はいちいちworld名を付けなければいけないのでちょっと面倒。