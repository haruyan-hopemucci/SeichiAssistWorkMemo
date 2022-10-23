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

## `zipped`問題

`(l1,l2,l3).zipped`みたいな構文で複数のコレクションをまとめてzipできるが、scala2.13からdeprecatedになった。

代わりに `lazyZip` を使えというアナウンスがあるが、`lazyZip`で作成されたViewは遅延評価対象で、for式のジェネレータを通しても評価が行われない。

ずっとこの現象で詰まっていて、最終的にzipをネストするしょうもない実装でPRしたが、`lazyZip`したviewに対して`toMap`すると評価されて中身が展開されることがわかった。

see: https://stackoverflow.com/questions/61551818/not-computed-on-collection-output-in-scala

SO万歳。

```scala
        val effectPrograms = for {
          ((blocks, lavas, waters), chunkIndex) <-
            (multiBreakList lazyZip
              multiLavaList lazyZip
              multiWaterList).zipWithIndex.toMap

```

https://github.com/scala/bug/issues/11041

によると、catsに`.parTupled`という`.zipped`とほぼ同様の動作をするメソッドがある。
こちらの方が直感的で望ましいとのコメントあり。

```scala
import cats.implicits._
(List(1,2,3), List("a", "b"), List(true, false)).parTupled 

-> val res2: List[(Int, String, Boolean)] = List((1,a,true), (2,b,false))
```
