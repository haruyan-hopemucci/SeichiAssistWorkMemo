# スキルで流体の水を消せるようにする #1643

## 変更箇所調査

src/main/scala/com/github/unchama/seichiassist/listener/PlayerBlockBreakListener.scala:189

```scala
          multiBreakList.addOne(breakBlocks.toSet)
          multiLavaList.addOne(lavaBlocks.toSet)
```
`multiLavaList`はスキル破壊対象の溶岩ブロックが入ったList(ArrayBuffer)

multiLavaListのもとになる溶岩ブロックのリスト `lavaBlocks` を作っている箇所
com/github/unchama/seichiassist/listener/PlayerBlockBreakListener.scala:149

```scala
          val BlockSearching.Result(breakBlocks, _, lavaBlocks) =
            BlockSearching
              .searchForBlocksBreakableWithSkill(player, breakArea.gridPoints(), block)
              .unsafeRunSync()
              .filterSolids(targetBlock =>
                isMultiTypeBreakingSkillEnabled || BlockSearching
                  .multiTypeBreakingFilterPredicate(block)(targetBlock)
              )
              .filterAll(targetBlock =>
                player.isSneaking || targetBlock
                  .getLocation
                  .getBlockY > playerLocY || targetBlock == block
              )

```
`BlockSearching.Result`の2引数目が匿名になっているが、ここには`searchForBlocksBreakableWithSkill`でしっかり水ブロックのリストが生成されている模様。
であれば、waterListもlavaListに混ぜてしまえば一気に消えるんじゃないか。
その際は`lavaList`という変数名では名称と中身が一致しないので`fluidList`みたいな名称に変更するか。

## 決めなければいけないこと

水1個分破壊するのに必要な耐久値。溶岩は10個分なので同じ計算にするか？