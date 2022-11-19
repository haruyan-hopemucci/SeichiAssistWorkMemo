# 棒メニューの鉱石交換にて鉱石ブロックでない鉱石も交換出来るように

## 現状の実装

### 公共デパートの鉱石買取人、および棒メニューの鉱石交換システム

```scala
    val requiredAmountPerTicket = Map(
      Material.COAL_ORE -> 128,
      Material.IRON_ORE -> 64,
      Material.GOLD_ORE -> 8,
      Material.LAPIS_ORE -> 8,
      Material.DIAMOND_ORE -> 4,
      Material.REDSTONE_ORE -> 32,
      Material.EMERALD_ORE -> 4,
      Material.QUARTZ_ORE -> 16
    )
```

これらは全部「鉱石」単位。

### 公共デパートの鉱物買取人

```
石炭ブロック　48個　　　　
鉄インゴット　64個
赤石ブロック　32個
ラピスラズリ　64個　
金インゴット　8個
ダイヤモンド　8個
ネザー水晶　64個　　＞　交換券　各1枚
```

このうち、ブロックにクラフトできる鉱物を棒メニューの交換対象に追加したいということと思われる。

対象は、

- 石炭
- レッドストーンダスト

の2つとなるか。

### 交換レート

鉱石と同じ（というよりブロックと同じ）で良いそうなので、

- 石炭432個　-> 交換券1枚
- レッドストーンダスト288個 -> 交換券1枚

となるだろう。
